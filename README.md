# Mike Harris · miketigerblue

Engineering leader and former CTO working across **Linux, networks, edge infrastructure and device trust**. My career spans Bermuda's early internet, carrier networks and certificate-authority infrastructure, through to secure connectivity for rail fleets at Motion Applied.

Alongside that work, I build an independent threat intelligence platform and practical AI integrations. The public projects below cover ingestion, structured enrichment, a SIP voice interface and a Proton Mail Bridge MCP server. The case studies show the decisions, operational problems and measurements behind the threat intelligence platform.

[LinkedIn](https://www.linkedin.com/in/tigerblue) · [Email](mailto:mike@tigerblue.tech)

> These are independent projects built outside my employment. They do not represent the systems of any current or former employer.

## ODIN — early prototype demo

A one-minute excerpt from an early handset test of my independent threat-intelligence voice interface. **Early prototype:** historical responses, retained as an engineering demonstration.

[![ODIN — early prototype. Silent captioned preview of a handset conversation about CVE-2024-3400, KEV and EPSS.](media/odin-early-prototype.gif)](https://github.com/miketigerblue/miketigerblue/blob/main/media/odin-early-prototype.mp4)

The captioned preview is silent. **[Video with audio (MP4)](https://github.com/miketigerblue/miketigerblue/raw/refs/heads/main/media/odin-early-prototype.mp4)** · [Transcript and prototype context](evidence/odin-early-prototype.md) · [ODIN code and runbook](https://github.com/miketigerblue/asterisk-twilio-pbx)

## Proton Mail Bridge MCP

**Email tools for Claude and other MCP clients.** A Python MCP server for searching, reading, drafting and organising email through Proton Mail Bridge.

The connection to Bridge uses local IMAP/SMTP, with certificate pinning, macOS Keychain credentials and pooled connections. The project includes installation guidance, troubleshooting and a documented threat model.

Community interest: **[8 stars](https://github.com/miketigerblue/proton-bridge-mcp/stargazers)** · **[4 forks](https://github.com/miketigerblue/proton-bridge-mcp/forks)**, as of 17 October 2026.

[**Repository and setup**](https://github.com/miketigerblue/proton-bridge-mcp) · [Threat model](https://github.com/miketigerblue/proton-bridge-mcp#threat-model)

## Engineering case studies

| Case study | What it demonstrates |
|---|---|
| **[Migrating a threat intelligence lake without losing its history](evidence/2026-05-lake-migration.md)** | Archive and restore design, compatibility problems, data-quality fixes, and honest handling of missing historical metadata. Migration: May 2026. |
| **[AI enrichment, measured in production](evidence/2026-10-07-13-enrichment.md)** | A dated processing snapshot with workload distribution, LLM-call latency and provenance-field coverage. Observation: 7–13 October 2026 UTC. |

## Evidence from production

A measured week from my independent platform: **1,982 analysis records across 271 completed recorded runs**, with model ID, prompt version and input hash present on every analysis in the window.

[![Production enrichment, 7–13 October 2026 UTC: 1,982 analyses, 271 completed recorded runs and 4.51-second p95 recorded LLM-call latency](diagrams/production-enrichment-2026-10-07-13.png)](evidence/2026-10-07-13-enrichment.md)

[Measurement definitions and case study](evidence/2026-10-07-13-enrichment.md) · [Aggregate data](evidence/2026-10-07-13-enrichment.json)

The figures describe recorded work and metadata. LLM-call latency is separate from end-to-end delivery; the snapshot does not establish uptime, maximum capacity or analysis accuracy.

## Public projects

| Project | Engineering focus | Read further |
|---|---|---|
| [**Tiger2Go**](https://github.com/miketigerblue/tiger2go) | Go ingestion service: concurrent workers, retries, normalisation, PostgreSQL migrations and operational metrics. | [System design](https://github.com/miketigerblue/tiger2go/blob/main/docs/SYSTEM_DESIGN.md) |
| [**Tiger-Eye**](https://github.com/miketigerblue/tiger-eye) | Python enrichment service: structured analysis, pgvector retrieval, bounded concurrency and execution metadata. | [Architecture and failure modes](https://github.com/miketigerblue/tiger-eye/blob/main/SYSTEM_DESIGN.md) |
| [**ODIN**](https://github.com/miketigerblue/asterisk-twilio-pbx) | Asterisk/SIP and a streaming voice bridge: audio pacing, interruptions, backpressure and tool-backed intelligence retrieval. | [Operational runbook](https://github.com/miketigerblue/asterisk-twilio-pbx/blob/main/RUNBOOK.md) |
| [**Proton Mail Bridge MCP**](https://github.com/miketigerblue/proton-bridge-mcp) | Python MCP server: local IMAP/SMTP integration, certificate pinning, Keychain credentials and pooled connections. | [Setup and threat model](https://github.com/miketigerblue/proton-bridge-mcp#threat-model) |

### How the platform evolved

The original platform used a Rust ingestor and a Fly.io-hosted lake, with L1 analysis and Python-based Number 2 reporting. In May 2026 I archived that lake and restored its threat intelligence data into a separate `legacy` schema alongside the newer Go-based Tiger2Go and Python Tiger-Eye stack. The migration record identifies T2 as the Go successor to Number 2.

That history matters when reading older diagrams and reports. The [migration case study](evidence/2026-05-lake-migration.md) distinguishes the old and new systems, and the repository documentation describes each public component. ODIN's documented Raspberry Pi/Fly.io voice deployment is separate from the retired Fly.io database.

[Historical ODIN report: 26 February–4 March 2026](ODIN-TLR-26-Feb--04-Mar-2026.pdf)

## Career Background

My career began in the early internet era.

I started in **BerMUG** in the late 1980s running a FirstClass BBS federated with global Mac communities.

In 1994 I helped build Bermuda's first commercial ISP, **Internet Bermuda Ltd**, serving as .BM TLD hostmaster and deploying one of the island's first secure online credit-card systems.

Later I became CTO of **Quantum Communications**, launching Bermuda's first metro-Ethernet service, CLEC interconnection, and a national IP/MPLS backbone.

My telecom background also includes work with **Cable & Wireless** and **Logic**, extending that experience across Bermuda's carrier and connectivity landscape.

At **QuoVadis** I designed and delivered a Tier III data centre supporting PKI certificate-authority infrastructure including HSM-backed systems.

I later founded **OnXis**, a security consultancy delivering incident response, digital forensics, and resilience engineering for governments and enterprises.

More recently I worked at **deltaflare**, building DevSecOps pipelines and hardened Linux systems for industrial IoT platforms.

Today I work at **Motion Applied**, engineering secure rail connectivity systems and supporting compliance work across ISO 27001, ISO 9001, ISO 14001, NIS2, and IEC 62443 environments.

<details>
<summary>Technical background and tools</summary>

## Tech Stack

### Languages

![Rust](https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)
![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=black)
![HTML/CSS](https://img.shields.io/badge/HTML%2FCSS-E34F26?style=flat-square&logo=html5&logoColor=white)

### AI / Machine Learning

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-API%20%2B%20Realtime-412991?style=flat-square&logo=openai&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-orchestration-1C3C3C?style=flat-square)
![HuggingFace](https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-Transformers-FFD21E?style=flat-square)
![Whisper](https://img.shields.io/badge/Whisper-ASR-412991?style=flat-square)
![pyannote.audio](https://img.shields.io/badge/pyannote-diarisation-orange?style=flat-square)
![RAG](https://img.shields.io/badge/RAG-vector--retrieval-lightgrey?style=flat-square)
![CNN](https://img.shields.io/badge/CNN-vision-EE4C2C?style=flat-square)
![SupCon](https://img.shields.io/badge/SupCon-contrastive--loss-EE4C2C?style=flat-square)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white)
![MPS](https://img.shields.io/badge/Apple%20MPS-GPU%20accel-A2AAAD?style=flat-square&logo=apple&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![MCP](https://img.shields.io/badge/MCP-Model%20Context%20Protocol-1C3C3C?style=flat-square)

### Networking & Carrier

![BGP](https://img.shields.io/badge/BGP-multi--AS%20routing-1B4F72?style=flat-square)
![IP/MPLS](https://img.shields.io/badge/IP%2FMPLS-backbone-1B4F72?style=flat-square)
![OSPF](https://img.shields.io/badge/OSPF-IGP-1B4F72?style=flat-square)
![Metro Ethernet](https://img.shields.io/badge/Metro--Ethernet-carrier-1B4F72?style=flat-square)
![CLEC](https://img.shields.io/badge/CLEC-interconnect-1B4F72?style=flat-square)
![DNS / TLD](https://img.shields.io/badge/DNS-TLD%20%2F%20BIND-1B4F72?style=flat-square)
![IPv6](https://img.shields.io/badge/IPv6-dual--stack-1B4F72?style=flat-square)
![NetFlow](https://img.shields.io/badge/NetFlow%2FsFlow-telemetry-1B4F72?style=flat-square)
![Wireshark](https://img.shields.io/badge/Wireshark-pcap-1679A7?style=flat-square&logo=wireshark&logoColor=white)

### PKI & Cryptography

![PKI](https://img.shields.io/badge/PKI-X.509%20%2F%20CA-003366?style=flat-square)
![HSM](https://img.shields.io/badge/HSM-key%20management-003366?style=flat-square)
![TLS / mTLS](https://img.shields.io/badge/TLS%2FmTLS-transport-003366?style=flat-square)
![OpenSSL](https://img.shields.io/badge/OpenSSL-721412?style=flat-square&logo=openssl&logoColor=white)
![JWT / JWKS](https://img.shields.io/badge/JWT-RS256%20%2F%20JWKS-000000?style=flat-square&logo=jsonwebtokens&logoColor=white)
![SRTP](https://img.shields.io/badge/SRTP-media%20security-003366?style=flat-square)
![1Password](https://img.shields.io/badge/1Password-vault-0094F5?style=flat-square&logo=1password&logoColor=white)

### Voice & Comms

![Asterisk](https://img.shields.io/badge/Asterisk-PJSIP-orange?style=flat-square)
![Twilio](https://img.shields.io/badge/Twilio-F22F46?style=flat-square&logo=twilio&logoColor=white)
![SIP/TLS](https://img.shields.io/badge/SIP%2FTLS-SRTP-lightgrey?style=flat-square)
![OpenAI Realtime](https://img.shields.io/badge/OpenAI-Realtime%20API-412991?style=flat-square&logo=openai&logoColor=white)

### Infrastructure & Data

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![pgvector](https://img.shields.io/badge/pgvector-embeddings-4169E1?style=flat-square)
![PostgREST](https://img.shields.io/badge/PostgREST-API-4169E1?style=flat-square)
![ChromaDB](https://img.shields.io/badge/ChromaDB-vector--store-orange?style=flat-square)
![Snowflake](https://img.shields.io/badge/Snowflake-analytics-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-cloud-232F3E?style=flat-square&logo=amazonaws&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-cloud-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![IONOS](https://img.shields.io/badge/IONOS-hosting-003D8F?style=flat-square)
![OVHcloud](https://img.shields.io/badge/OVHcloud-infrastructure-123F6D?style=flat-square)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Fly.io](https://img.shields.io/badge/Fly.io-deploy-purple?style=flat-square)
![Cloudflare](https://img.shields.io/badge/Cloudflare-Workers%20%2F%20D1-F38020?style=flat-square&logo=cloudflare&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-IaC-7B42BC?style=flat-square&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-IaC-EE0000?style=flat-square&logo=ansible&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![MQTT](https://img.shields.io/badge/MQTT-pub%2Fsub-660066?style=flat-square&logo=mqtt&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-edge-A22846?style=flat-square&logo=raspberrypi&logoColor=white)
![nginx](https://img.shields.io/badge/nginx-009639?style=flat-square&logo=nginx&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-metrics-E6522C?style=flat-square&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-metrics-E6522C?style=flat-square&logo=grafana&logoColor=white)
![Loki](https://img.shields.io/badge/Loki-logs-E6522C?style=flat-square)
![Linux](https://img.shields.io/badge/Linux-hardened-FCC624?style=flat-square&logo=linux&logoColor=black)

### Security & Intelligence

![OSINT](https://img.shields.io/badge/OSINT-NVD%2FCISA%2FEPSS-orange?style=flat-square)
![CTI](https://img.shields.io/badge/CTI-threat--intelligence-red?style=flat-square)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-CC0000?style=flat-square)
![STIX/TAXII](https://img.shields.io/badge/STIX%2FTAXII-CTI%20exchange-CC0000?style=flat-square)
![MISP](https://img.shields.io/badge/MISP-threat--share-darkred?style=flat-square)
![YARA](https://img.shields.io/badge/YARA-detection-darkred?style=flat-square)
![Sigma](https://img.shields.io/badge/Sigma-detection-darkred?style=flat-square)
![DFIR](https://img.shields.io/badge/DFIR-incident%20response-CC0000?style=flat-square)
![SIEM](https://img.shields.io/badge/SIEM-log%20analytics-CC0000?style=flat-square)
![ICS / OT](https://img.shields.io/badge/ICS%2FOT-security-FF6B35?style=flat-square)
![DevSecOps](https://img.shields.io/badge/DevSecOps-shift--left-darkgreen?style=flat-square)

### Compliance & Audit

![ISO 27001](https://img.shields.io/badge/ISO-27001-blue?style=flat-square)
![ISO 9001](https://img.shields.io/badge/ISO-9001-blue?style=flat-square)
![ISO 14001](https://img.shields.io/badge/ISO-14001-green?style=flat-square)
![NIS2](https://img.shields.io/badge/NIS2-compliance-lightgrey?style=flat-square)
![IEC 62443](https://img.shields.io/badge/IEC%2062443-OT%20security-FF6B35?style=flat-square)
![GDPR](https://img.shields.io/badge/GDPR-data%20protection-blue?style=flat-square)
![PCI DSS](https://img.shields.io/badge/PCI%20DSS-payment%20security-blue?style=flat-square)

</details>

## Get in Touch

- **LinkedIn:** [linkedin.com/in/tigerblue](https://www.linkedin.com/in/tigerblue)
- **X:** [@LoveFlowState](https://x.com/loveflowstate)
- **Email:** mike@tigerblue.tech
