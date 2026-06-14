# Halo, saya Hary Prihadi 👋

> Fullstack Developer yang berfocus pada Backend & Systems

&emsp;&emsp;Selamat datang di ruang kerja digital saya.

&emsp;&emsp;Saya adalah seorang **Fullstack developer** yang berfokus pada **Backend** yang mengutamakan skalabilitas, efisiensi sistem, dan otomatisasi. Saat ini saya aktif membangun backend architectures modern, sembari mengeksplorasi ekosistem Nix, administrasi sistem Linux, dan integrasi kecerdasan buatan (AI) pada sisi server.

## Core Tech Stack & Fokus Saat Ini

Backend Development

- **Express.js** & **JavaScript** (Core Saat Ini): Saya membangun backend services yang robust, `RESTful`, dan `event-driven`. Fokus saya adalah pada `clean architecture`, `middleware design`, `database migrations`, dan menulis kode yang predictable serta maintainable.

- Transisi ke **TypeScript** (Mendatang): Menyadari manfaat keamanan yang luar biasa dari static analysis, saya sedang mentransisikan proyek-proyek **JavaScript** saya ke **TypeScript.** Tujuan saya adalah membangun type-safe architectures yang tangguh guna mencegah bug saat runtime sebelum masuk ke production.

Systems & Environments

- **Linux:** Operating system ini adalah kanvas utama saya dalam development dan deployment. Saya sangat terbiasa dengan terminal-centric workflows, bash scripting, dan performance optimization.

- **Nix** & **NixOS:** Saya sangat percaya pada absolute reproducibility. Saya menggunakan **Nix** untuk mengelola development shells dan system configurations. Dengan mendeklarasikan konfigurasi sistem secara deklaratif di **NixOS**, saya mengeliminasi masalah klasik "_works on my machine_" sepenuhnya.

## Future Horizons & Pembelajaran Aktif

&emsp;&emsp;Saya selalu berusaha melampaui batas dari apa yang saya ketahui. Saat ini, saya sedang aktif memfokuskan proses belajar mandiri saya pada tiga lini utama:

1. Systems Languages: **Go** (Golang)

   Meskipun **Node.js** sangat bagus untuk rapid development, saya sedang memperluas toolkit saya dengan mempelajari **Go**. Saya tertarik pada:
   - Concurrency model yang luar biasa (`Goroutines` dan `Channels`)
   - Compilation times yang sangat cepat
   - Single-binary deployments yang berjalan sangat baik di containerized environments

1. Intelligent Backends: **LLM Integrations**

   AI sedang mendefinisikan ulang user experiences, dan keajaiban itu terjadi di backend. Saya sedang mempelajari cara mengintegrasikan Large Language Models (LLMs) ke dalam backend architectures secara mulus, dengan fokus pada:
   - Vector Databases (seperti `pgvector`, `Pinecone`, atau `Qdrant`) untuk Semantic Search
   - Membangun Retrieval-Augmented Generation (RAG) pipelines yang andal
   - Agentic orchestration frameworks (`LangChain`, `LlamaIndex`) yang diterjemahkan ke dalam API endpoints yang robust

1. DevOps & Infrastructure Engineering

   Bagi saya, kualitas kode sama pentingnya dengan kualitas deployment-nya. Saya sedang membangun keahlian dalam:
   - Containerization & Orchestration: `Docker`, `Podman`, dan distribusi Kubernetes ringan (`K3s`).
   - CI/CD Pipelines: `GitHub Actions` dan `GitLab CI`, yang diotomatisasi dengan `Nix flakes`.
   - Infrastructure as Code (IaC): Menggabungkan kekuatan Terraform dengan Nix-based deployments (_in the future_).

## Portfolio Projects

Di bawah ini adalah beberapa konsep dan proyek yang sedang saya kembangkan.

- Project Name: [AI Learning Planner](./capstone_ai_learning_planner.md)

  Sebuah website yang menghasilkan rencana belajar mingguan yang dipersonalisasi, berdasarkan konteks nyata dari database, bukan template statis.

  Tech:
  - **Backend:** Node.js + Express.js (layered architecture: routes → models → services)
  - **AI:** Google Gemini 2.5 Flash via `@google/generative-ai`
  - **Database:** PostgreSQL 16 + `node-pg-migrate` + JSONB untuk output AI
  - **Validation:** Zod (schema-first, ketat di server)
  - **Auth:** JWT (access + refresh token)
  - **Observability:** Pino (logging), prom-client (metrics), custom error classes

## Get in Touch

&emsp;&emsp;Saya selalu terbuka untuk berkolaborasi dalam backend systems, proyek open-source Nix, atau DevOps automation. Silakan hubungi saya!

- GitHub: [@mocbaltig](https://github.com/mocbaltig)
- LinkedIn: [Hary Prihadi](https://www.linkedin.com/in/hary-prihadi/)
- Email: <phdhary@gmail.com>

> _"Environments terbaik adalah yang bersifat declarative dan systems terbaik adalah yang bersifat reproducible."_ — someone said, can't remember who though.
