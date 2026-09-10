# sentinel-DPI-LLM
A high-performance, multithreaded Deep Packet Inspection (DPI) engine in Python integrated with a self-hosted Ollama LLM for real-time threat detection and dynamic rule generation. Fully containerized with Docker
# * SentinelDPI-LLM: AI-Powered Deep Packet Inspection Engine

**SentinelDPI-LLM** is a next-generation, high-throughput Deep Packet Inspection (DPI) and traffic control system built in Python. Unlike traditional static firewalls, it combines multithreaded 5-tuple network packet parsing with an integrated, self-hosted Local LLM (Ollama) to dynamically analyze traffic anomalies and automatically inject real-time security rules without engine downtime.

---

##  Key Features

- ** Deep Packet Inspection (DPI):** Parses `.pcap` network captures to extract 5-Tuple flow identifiers (Source/Dest IP, Source/Dest Port, Protocol) and Server Name Indication (SNI) [1, 2].
- **Multithreaded Processing:** Implements Load Balancer and Fast Path worker threads for parallel packet processing and connection grouping [3, 4].
- ** LLM-Driven Threat Intelligence:** Integrates with a local **Ollama LLM** (e.g., `phi3` / `llama3`) to analyze suspicious network logs and dynamically auto-generate JSON-formatted blocking rules.
- ** Live In-Memory Rule Engine:** Dynamically ingests LLM-generated rules on the fly to drop malicious packets or enforce rate-limiting without restarting the application [4, 5].
- **Fully Containerized (Docker & Compose):** Features a microservice architecture (`dpi-engine` + `ollama`) with an automated entrypoint script to pull AI models on deployment.

---

##  Microservices Architecture

```text
[ PCAP Traffic Input ]
          │
          ▼
┌─────────────────────────────────────────────────────────────┐
│                   DOCKER COMPOSE NETWORK                    │
│                                                             │
│  ┌──────────────────────────┐    HTTP REST    ┌──────────┐  │
│  │   python-dpi-engine      │ ──────────────► │  ollama  │  │
│  │                          │                 │  (LLM)   │  │
│  │ • 5-Tuple & SNI Parser   │ ◄────────────── │          │  │
│  │ • In-Memory Rule Store   │   JSON Rules    └──────────┘  │
│  │ • Traffic Filtering      │                               │
│  └──────────────────────────┘                               │
└─────────────────────────────────────────────────────────────┘
