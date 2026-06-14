# Capstone AI Learning Planner

Portfolio Case Study of AI Learning Planner

[Link Repository](https://github.com/mocbaltig/learning-planner-ai)

## Problem

Peserta bootcamp Dicoding memiliki tujuan belajar yang jelas — misalnya "kuasai React dalam 4 minggu" — tetapi secara konsisten gagal menerjemahkannya ke jadwal mingguan yang realistis. Tiga akar masalah:

1. **Decomposition gap** — Goal besar seperti "pelajari React" tidak terpecah menjadi sesi belajar yang konkret (hari, durasi, slot waktu). Peserta tahu _apa_ yang ingin dicapai tapi tidak _bagaimana_ memulainya minggu ini.

2. **Capacity blindness** — Rencana dibuat tanpa mempertimbangkan kapasitas nyata minggu tersebut. Jadwal terlalu padat karena mengabaikan task yang sudah ada di minggu yang sama, sehingga cepat ditinggalkan.

3. **No adaptive feedback loop** — Saat satu sesi terlewat, tidak ada mekanisme penyesuaian otomatis. Peserta cenderung menyerah pada seluruh rencana mingguan alih-alih menjadwal ulang.

Dampak: tingkat penyelesaian rencana mingguan rendah, bukan karena kurang motivasi, tetapi karena sistem perencanaan tidak membantu peserta memulai dengan langkah yang realistis.

## Approach

Solusi: **AI-powered weekly planning engine** yang menghasilkan rencana belajar personal berdasarkan konteks nyata dari database, bukan template statis.

### Arsitektur

```
Client (React) → Express REST API → Gemini 2.5 Flash
                      ↕
              PostgreSQL (9 tables)
```

### Alur Suggest (inti)

```
POST /api/ai/plan/suggest
  1. Server ambil goal + profile + existing tasks dari DB
  2. Kirim context ke Gemini 2.5 Flash
  3. Validasi output JSON dengan Zod
  4. Retry 1x jika output tidak valid (90%+ lolos percobaan pertama)
  5. Simpan ke ai_recommendations (audit trail)
  6. Kembalikan ke client — user memutuskan accept/reject
```

### Alur Reschedule (adaptif)

```
POST /api/ai/plan/reschedule
  1. Terima task_ids yang overdue
  2. Hitung remaining_capacity = target_jam - completed_jam (real-time)
  3. Kirim context + occupied_slots ke Gemini
  4. LLM hindari konflik slot secara native (tanpa post-validation)
  5. Simpan rekomendasi + audit trail
```

### Trade-off Keputusan Teknis

| Keputusan                                    | Opsi Ditolak                 | Alasan                                                                                                          |
| -------------------------------------------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Gemini 2.5 Flash** (gratis)                | OpenAI/Anthropic (berbayar)  | Capstone tanpa anggaran API. Konsekuensi: vendor lock-in, rate limit 15 req/menit.                              |
| **Zod validation + retry**                   | Percaya output LLM mentah    | LLM tidak selalu menghasilkan JSON valid. Retry 1x menangani fluktuasi tanpa latency berlebih.                  |
| **Human-in-the-loop**                        | Auto-execute saran AI        | User tetap punya kontrol penuh. AI assist, bukan replace.                                                       |
| **PostgreSQL + JSONB**                       | MongoDB (dokumen murni)      | Relasi kuat (FK, CASCADE) + data semi-terstruktur (output AI) dalam satu DB. Trade-off: dependency Docker.      |
| **Eager progress recalculation**             | Lazy/cron-based              | Akurasi real-time penting untuk context AI (`remaining_capacity`). Trade-off: overhead ~5-15ms per mutasi task. |
| **Privacy-first context** (tanpa email/nama) | Kirim full user data ke LLM  | Data pribadi tidak perlu diketahui AI untuk membuat jadwal. Default, bukan opt-in.                              |
| **Mock mode** (`LLM_PROVIDER=mock`)          | Selalu panggil API sungguhan | Isolasi development + testing tanpa kuota API. Trade-off: behavior bisa berbeda dari production.                |

### Stack

- **Backend:** Node.js + Express.js (layered architecture: routes → models → services)
- **AI:** Google Gemini 2.5 Flash via `@google/generative-ai`
- **Database:** PostgreSQL 16 + `node-pg-migrate` + JSONB untuk output AI
- **Validation:** Zod (schema-first, ketat di server)
- **Auth:** JWT (access + refresh token)
- **Observability:** Pino (logging), prom-client (metrics), custom error classes

## Impact

### Metrik Target

| Metrik                    | Target                             | Cara Ukur                                                                        |
| ------------------------- | ---------------------------------- | -------------------------------------------------------------------------------- |
| Acceptance rate saran AI  | ≥ 50%                              | Rasio `status = 'accepted'` di tabel `ai_recommendations`                        |
| Valid output rate LLM     | ≥ 90%                              | Output lolos Zod di percobaan pertama (log `ai_suggest_failed` vs total request) |
| Latency endpoint suggest  | Median < 5 detik                   | Request log Pino + prom-client metrics                                           |
| Token cost accountability | Terdokumentasi per 100 rekomendasi | Endpoint `GET /api/ai/token-usage` untuk monitor biaya operasional               |

### Dampak Kualitatif

- **Dari blank canvas ke concrete starting point** — Peserta tidak perlu merancang jadwal dari nol. AI memberikan draft yang bisa langsung diedit atau disetujui.
- **Jadwal yang realistis** — Rencana mempertimbangkan existing tasks dan kapasitas nyata minggu tersebut, mengurangi over-scheduling.
- **Adaptif terhadap keterlambatan** — Fitur reschedule memungkinkan penyesuaian otomatis saat task terlewat, tanpa menunggu minggu depan.
- **Data-driven iteration** — Setiap rekomendasi (accept/reject) tersimpan sebagai audit trail, membuka peluang fine-tuning prompt berdasarkan pola user di iterasi berikutnya.

### Lessons Learned

1. **LLM output validation is non-negotiable** — Zod + retry pattern mencegah data invalid masuk database. Tanpa ini, sistem akan menyimpan garbage yang merusak UX.
2. **Mock mode mempercepat iterasi** — Development fitur AI tanpa mock mode berarti setiap perubahan prompt menghabiskan kuota API dan menunggu 2-5 detik. Mock mode memungkinkan loop dev < 1 detik.
3. **Human-in-the-loop builds trust** — Pengguna lebih percaya pada saran AI ketika mereka bisa menolak tanpa konsekuensi. Acceptance rate yang tinggi adalah bukti, bukan paksaan.
4. **Capacity awareness prevents drop-off** — Sumber utama kegagalan rencana belajar bukan materi sulit, tapi jadwal terlalu padat. Dengan memasukkan existing tasks ke context AI, resiko over-scheduling berkurang signifikan.
