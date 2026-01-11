# 🚀 Legal Policy Extraction & Chat System

An **end-to-end system** that automatically discovers company legal policy pages, extracts structured information using an LLM, and exposes the results via a **minimal PHP chat interface**. Fully compatible with the **n8n AI Starter Kit** stack (n8n, Ollama, Qdrant, PostgreSQL).

---

## 🖼️ Preview

<p align="center">
  <img src="https://github.com/user-attachments/assets/be38c513-49cd-4455-8736-8c4c6c3d5b4c" alt="Image" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/900161e4-2cea-4735-8919-2426a1357a3f" alt="Image 1" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/0796f847-6733-425a-ae90-bfa813bdd3b7" alt="Image 2" width="600"/>
</p>


---

## 🧩 Project Overview

**Goal:** Build a reproducible system to:

* Discover legal policy pages for companies/domains.
* Scrape, clean, and store policy text in a vector DB.
* Extract structured scope data using an LLM into SQL.
* Expose structured + unstructured data via a PHP chat interface.

---

## 📂 Provided Files

| File              | Description                                                                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `List 1.csv`      | Company/domains input list with fields (`id`, `name`, `domain`, `generic_email`, `contact_email`, `privacy_email`, `delete_link`, `country`). Some fields may be null. |
| `Template 1.xlsx` | Policy-scope matrix for structured extraction. `SCOPES` column defines standardized categories; `Company #1, Company #2…` indicate scope applicability.                |

---

## ⚙️ Core Objectives

**Objective 1 — Policy Page Discovery**

* Automatically discover **Privacy Policy** and **Terms & Conditions** pages.
* Use crawling + heuristics (footer links, sitemap, link text).
* Output canonical URLs with metadata.

**Objective 2 — Scraping and Vector Storage**

* Scrape discovered pages and clean/chunk text.
* Store chunks in **Qdrant** vector database with metadata:

  * `id` (from List 1)
  * `domain`
  * `doc_type` (privacy | terms)
  * `source_url`

**Objective 3 — LLM Parsing into SQL (Template 1)**

* Use an LLM to retrieve relevant chunks.
* Map Template 1 scopes to structured fields in **PostgreSQL**.
* Validate output via JSON → SQL insert/upsert.

**Objective 4 — PHP Chat Interface**

* Minimal PHP page with chat window.
* LLM queries vector DB + SQL DB for responses.
* Responses include source URLs.

---

## 🛠️ Technical Stack

| Layer               | Technology      | Purpose                          |
| ------------------- | --------------- | -------------------------------- |
| Crawling & Scraping | Python          | Discover and scrape policy pages |
| LLM Parsing         | Python + Ollama | Extract structured policy scopes |
| Vector Storage      | Qdrant          | Store and query policy text      |
| SQL Storage         | PostgreSQL      | Template 1 structured data       |
| Orchestration       | n8n             | Workflow automation & scheduling |
| Chat Interface      | PHP             | Minimal UI for querying policies |

---

## 📦 Setup & Installation

> Compatible with **n8n AI Starter Kit**.

1. **Create environment file**

```bash
cp .env.example .env
```

* Update credentials, DB passwords, API keys.

2. **Start services**

```bash
# CPU only
docker compose --profile cpu up -d

# NVIDIA GPU
docker compose --profile gpu-nvidia up -d

# AMD GPU (Linux)
docker compose --profile gpu-amd up -d
```

3. **Access Services**
   | Service | URL |
   |--------|-----|
   | n8n Editor | [http://localhost:5678](http://localhost:5678) |
   | PostgreSQL | localhost:5432 |
   | Qdrant UI | [http://localhost:6333](http://localhost:6333) |
   | Ollama API | [http://localhost:11434](http://localhost:11434) |
   | PHP Chat | [http://localhost:8080](http://localhost:8080) |

---

## 🗂️ Data Flow

1. Input: `List 1.csv` → domains
2. Discovery: Python crawler → canonical URLs
3. Scraping: clean & chunk text
4. Vector Storage: Qdrant → store text + metadata
5. LLM Parsing: Template 1 scopes → PostgreSQL
6. Query: PHP chat interface → LLM → vector + SQL → response

---

## 📄 Deliverables

* Source repo with README
* Docker Compose / n8n integration
* SQL schema + migration scripts
* Example output for 3+ domains
* Engineering notes (trade-offs, limitations)

---

## 🔑 Evaluation Focus

* Correct mapping: policy text → Template 1 scopes
* Reliable policy page discovery
* End-to-end reproducibility

---

## 🔁 Updating / Re-running

```bash
docker compose pull
docker compose create
docker compose up -d
```

* n8n schedules automatic workflow runs
* Python scripts can run independently for batch updates

---

## 📜 License

Apache License 2.0 — See LICENSE

---

## 💬 Support & Community

* n8n Forum: [https://community.n8n.io](https://community.n8n.io)
* Workflow templates: [https://n8n.io/workflows](https://n8n.io/workflows)
