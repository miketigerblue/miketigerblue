# Mike Harris · miketigerblue

[![Profile Views](https://komarev.com/ghpvc/?username=miketigerblue&style=flat-square&color=blue)](https://github.com/miketigerblue)
[![GitHub followers](https://img.shields.io/github/followers/miketigerblue?style=flat-square&label=Followers)](https://github.com/miketigerblue?tab=followers)
[![Blog](https://img.shields.io/badge/Blog-tigerblue.tech-informational?style=flat-square&logo=ghost)](https://tigerblue.tech)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-tigerblue-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/tigerblue)

> I build systems that sit at the ugly intersection of legacy protocols and modern AI — then make them production-grade.

Thirty years across the infrastructure stack — starting as a founding technical contributor to Bermuda's first commercial ISP (**IBL**, 1994: .BM TLD hostmaster, DEC Alpha backbone, Bermuda's first secure online credit card systems). Rose to CTO at national carrier **Quantum Communications**, launching Bermuda's first metro-Ethernet service, CLEC interconnection, carrier-grade VoIP, and a national IP/MPLS backbone. At **QuoVadis**, designed and delivered a $10m Tier-III data centre from bare floor to PKI-grade production — Arista 7124SX MLAG, SolidFire all-SSD clusters, ZFS, HSMs, dark fibre rings, and a seamless migration of all PKI backend systems. Founded and ran **OnXis**, a boutique consultancy delivering IT/DR, incident response, and digital forensics for governments and enterprises across 13 years. First technical hire at cybersecurity start-up **deltaflare**, building DevSecOps pipelines, PKI microservices, and hardened real-time Linux kernels for factory edge appliances. Now at **Motion Applied**, leading rail connectivity engineering: porting the F1-derived **FleetConnect v3** stack from x86 to three-node ARM cluster, delivering North America's first rail-certified **Starlink SATCOM** deployments, and serving as ISO 27001/9001/14001 internal auditor and Security Champion.

These days I'm focused on a problem that shouldn't be hard: making cyber threat intelligence actually useful to the people who need to act on it — fast, auditable, and without a dashboard in sight.

The current stack is a vertically integrated CTI pipeline: **tigerfetch** (private Rust) ingests raw feeds into a PostgreSQL-backed OSINT platform; **L1**, an automated cyber analyst microservice, enriches each article with GPT-5-mini + RAG, producing structured threat assessments; **Number 2** reads that enriched output and clusters it into hourly SITREPs; and **Project Odin** delivers those SITREPs by voice over a SIP phone — a real analyst picking up a real handset and talking to an AI SOC analyst backed by live intelligence. **Tiger2Go** is the open-source Go port of tigerfetch.

---

## ⚒️ Active Projects

![Odin Realtime Architecture](odin-realtime-architecture.png)
*Project Odin — realtime voice AI architecture: SIP PBX → Twilio → WebSocket bridge → OpenAI Realtime, backed by a cached threat-intel data plane.*

| Project | Stack | Status |
|---|---|---|
| 🔵 **OSINT Platform + L1** | Python · PostgreSQL · ChromaDB · FastAPI · TypeScript · Fly.io | Active |
| 🏛️ **Project Odin** | Asterisk · Twilio · Node.js · OpenAI Realtime · PostgREST · Fly.io | Active |
| 2️⃣ **Number 2** | Python · PostgreSQL · pgvector · OpenAI · PostgREST · Fly.io | Active |
| 🐯 [**Tiger2Go**](https://github.com/miketigerblue/tiger2go) | Go · PostgreSQL · pgx · Prometheus · goose | Active |

### OSINT Platform + L1 Cyber Analyst

**tigerfetch** (private Rust, `rust-feed-ingestor`) continuously ingests from **20 RSS/Atom feeds** (NCSC, CISA, JPCERT, MISP, BleepingComputer, Unit 42, Exploit-DB, and others) into a PostgreSQL OSINT platform. **L1** — the OSINT Enricher microservice — then acts as an automated cyber analyst: it reads each new article, retrieves top-3 semantically similar past analyses from ChromaDB (RAG), enriches using **GPT-5-mini**, and persists 17-field structured assessments — severity, confidence, CVEs, TTPs, IOCs, threat actors, target geographies, recommended actions — back to PostgreSQL and the vector store.

**Operational stats (102 days continuous service):** 3,103 enriched analyses · 4.5M EPSS daily scores · 1,240 hourly SITREPs generated · 2.4 GB database.

**Access layers:**
- **FastAPI REST API** — 9 routers exposing analyses, CVE catalogue, EPSS data, and sitrep feeds
- **[osint-enricher-mcp](https://github.com/miketigerblue/osint-enricher-mcp)** (TypeScript) — 8 tools consumed by VS Code Copilot, Claude Desktop, and GPT agents: `health_check`, `list_analysis`, `get_analysis`, `semantic_search`, `summarise_rss_feed`, `compare_cve_to_rss`, `get_cve_detail`, `get_raw_cve_detail`
- **Frontend dashboards** — interactive analysis browser, cyber risk dashboards, geopolitical timelines, monthly reporting
- **Snow-Tiger** — delta export pipeline: PostgreSQL → S3 (Parquet) → Snowflake, with EPSS/KEV enrichment and SBOM ingestion (CycloneDX/SPDX)

### Project Odin

A vertically integrated voice AI system that turns a classic SIP PBX into a natural-language interface for a cyber threat-intelligence workflow. A human operator picks up a LAN phone, dials an extension, and reaches **ODIN** — an AI-powered SOC analyst that delivers real-time cyber SITREPs, looks up CVEs, and performs semantic threat searches, entirely by voice.

The architecture separates into two planes:

**Voice / SIP plane (latency-critical):** LAN handsets register to Asterisk (PJSIP) → outbound over SIP/TLS + SRTP to Twilio → Twilio webhook returns TwiML `<Connect><Stream>` → a Node.js WebSocket bridge on Fly.io forwards bidirectional G.711 μ-law audio to the **OpenAI Realtime API**, with 20ms frame pacing, barge-in detection, backpressure management, and gated tool-call execution.

**Threat-intel data plane (near-real-time, cached):** OSINT ingestion from RSS/Atom feeds + NVD/CISA KEV/EPSS enrichment → normalisation, severity tagging, and rolling 24h SITREP generation → served via **PostgREST** and the OSINT platform's semantic search API (ChromaDB). The bridge pre-fetches and caches SITREP context to keep first-audio latency bounded.

Deployed to a **Raspberry Pi** (Asterisk in Docker, provisioned via Ansible) and **Fly.io** (bridge, PostgREST). Validated with real Wireshark captures, annotated live call transcripts, and an operational runbook.

### Number 2

An automated, hourly cyber threat-intelligence SITREP pipeline. Every hour, Number 2 reads the structured, enriched analysis output produced by the L1 Cyber Analyst, then runs it through a three-stage pipeline to produce an auditable intelligence report — published via PostgREST and consumed by Project Odin's voice interface.

**Pipeline stages:**

- **Stage 1 (extraction):** Each item is normalised to a schema-validated object — item type, CVEs, actors, TTPs, attack vectors, IOCs, and an `embedding_text` used for clustering. Falls back to deterministic heuristics when LLM is unavailable.
- **Embedding cache:** Vectors are computed once per item (OpenAI embeddings) and cached in `pgvector` — subsequent runs reuse them to keep cost and latency bounded.
- **Hybrid clustering:** Agglomerative cosine clustering groups semantically similar items; a deterministic rule layer merges clusters sharing the same CVE or threat actor.
- **Scoring:** Each cluster receives a Priority Index (0–100) built from CVSS, EPSS, KEV status, actor recidivism, and TTP sophistication — fully deterministic, explainable, and tuneable.
- **Stage 2 (synthesis):** Top clusters are synthesised into analyst-ready `ClusterBrief` objects. Source provenance is preserved deterministically — the LLM cannot alter `source_items`.
- **Stage 3 (narrative):** A short executive summary highlights deltas vs the previous hour.

**Additional capabilities:** convergence detection (cross-signal pattern recognition across sector × geography × actor × TTP × malware dimensions, scored 0–100); stack watchlist alerting; daily/weekly/monthly CISO and SOC briefs via email; blog post generation (Astro MDX); and EPSS partition lifecycle management.

Runs on Fly.io via `supercronic`, with Pydantic v2 output validation throughout.

→ [**Sample Report: Threat Landscape 26 Feb – 04 Mar 2026**](ODIN%20Threat%20Landscape%20Report%20%E2%80%94%2026%20Feb%20to%2004%20Mar%202026.pdf)

### [Tiger2Go](https://github.com/miketigerblue/tiger2go)

An open-source Go port of **tigerfetch**, the private Rust ingestor at the core of the OSINT platform. The port direction is deliberate: the dominant problem in this system is not memory ownership, it's operational ingestion — high-volume I/O, untrusted inputs, concurrency, retries, and long-running process reliability. Go is simply the right shape for that.

Concurrent workers ingest **RSS/Atom** security feeds (sanitised via `bluemonday`), **NVD** CVE details (windowed 120-day chunks, v2 API), **CISA KEV** (Known Exploited Vulnerabilities catalogue), and **EPSS** daily scores (~300k records/day). Deduplication and idempotency are first-class; schema migrations run via embedded `goose`. Exposes `/metrics` (Prometheus) and `/healthz` for operational observability.

The layering philosophy: Rust for foundations, **Go for control planes**, Python for reasoning layers. Tiger2Go sits firmly in the middle.

---

## 🧰 Tech Stack

**Languages**
![Rust](https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)

**Voice & Comms**
![Asterisk](https://img.shields.io/badge/Asterisk-PJSIP-orange?style=flat-square)
![Twilio](https://img.shields.io/badge/Twilio-F22F46?style=flat-square&logo=twilio&logoColor=white)
![SIP/TLS](https://img.shields.io/badge/SIP%2FTLS-SRTP-lightgrey?style=flat-square)
![OpenAI Realtime](https://img.shields.io/badge/OpenAI-Realtime%20API-412991?style=flat-square&logo=openai&logoColor=white)

**Infrastructure & Data**
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![pgvector](https://img.shields.io/badge/pgvector-embeddings-4169E1?style=flat-square)
![PostgREST](https://img.shields.io/badge/PostgREST-API-4169E1?style=flat-square)
![ChromaDB](https://img.shields.io/badge/ChromaDB-vector--store-orange?style=flat-square)
![Snowflake](https://img.shields.io/badge/Snowflake-analytics-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Fly.io](https://img.shields.io/badge/Fly.io-deploy-purple?style=flat-square)
![Ansible](https://img.shields.io/badge/Ansible-IaC-EE0000?style=flat-square&logo=ansible&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-edge-A22846?style=flat-square&logo=raspberrypi&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-metrics-E6522C?style=flat-square&logo=prometheus&logoColor=white)

**Security**
![OSINT](https://img.shields.io/badge/OSINT-NVD%2FCISA%2FEPSS-orange?style=flat-square)
![CTI](https://img.shields.io/badge/CTI-threat--intelligence-red?style=flat-square)

---

## 📊 GitHub Stats

![Mike's GitHub Stats](https://github-readme-stats.vercel.app/api?username=miketigerblue&show_icons=true&theme=dark&hide_border=true&count_private=true)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=miketigerblue&layout=compact&theme=dark&hide_border=true)

---

## 📬 Get in Touch

- **LinkedIn:** [linkedin.com/in/tigerblue](https://www.linkedin.com/in/tigerblue)
- **X:** [@miketigerblue](https://x.com/miketigerblue)
- **Email:** mike@tigerblue.tech
- **Blog:** [tigerblue.tech](https://tigerblue.tech)
