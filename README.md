# 🧪 Agent Playground

A hands-on playground and reference repository for building, testing, and experimenting with autonomous AI agents using **Google ADK (Agent Development Kit)**, **`google/agents-cli`**, local LLMs (**Ollama / Gemma**), and the **Antigravity CLI (`agy-bridge`)**.

---

## 🎯 Overview

This repository houses experimental prototypes and production blueprints demonstrating:
- **Model-Agnostic Agent Orchestration**: Switching smoothly between cloud APIs (Google Gemini) and 100% offline, private on-device models (Ollama Gemma/Qwen or local OpenAI-compatible endpoints).
- **Tool Use & Function Calling**: Giving agents typed Python tools to interact with external APIs, timezones, and simulated environments.
- **Local Dev & Visualization**: Testing and debugging agent reasoning loops using the ADK Web Playground (`agents-cli playground`) and CLI runner.

---

## 📁 Repository Structure

```text
agentplayground/
├── demo/                       # Starter Google ADK Agent
│   ├── app/
│   │   ├── agent.py            # Agent definition, tool declarations, and model binding
│   │   ├── fast_api_app.py     # FastAPI backend serving ADK SSE endpoints
│   │   └── app_utils/          # A2A, services, and reasoning adapters
│   ├── tests/                  # Unit, integration, and eval datasets
│   ├── Dockerfile              # Container definition for cloud / microVM deployment
│   ├── agents-cli-manifest.yaml# Manifest for agents-cli toolchain
│   ├── pyproject.toml          # uv / Python dependencies
│   ├── GEMINI.md               # Context guidelines for AI coding assistants
│   └── README.md               # Detailed walkthrough for the demo agent
├── .gitignore                  # Security-first ignore rules (protects API keys & venvs)
└── README.md                   # Repository guide
```

---

## 🚀 Projects in this Playground

### 1. [`demo`](./demo/) — Multi-Backend ADK Starter Agent
* **Tools**: `get_weather` and `get_current_time`.
* **Supported Backends**:
  * 🌐 **Google Gemini** (`gemini-3.7-flash` via `google-genai` and `GEMINI_API_KEY`)
  * 💻 **Local Ollama** (`gemma4:12b-mlx` or `qwen2.5-coder:14b` on Apple Silicon GPU)
  * 🌉 **Antigravity CLI Proxy** (`openai/agy-gemini` via `agy-bridge`)
* See [demo/README.md](./demo/README.md) for step-by-step setup and code examples.

---

## 🛠️ Prerequisites

To run agents in this playground, ensure you have:
1. **Python 3.11+**
2. **`uv`**: Fast Python package manager (`brew install uv` or `curl -LsSf https://astral.sh/uv/install.sh | sh`)
3. **`agents-cli`**: Google Agents CLI (`uvx google-agents-cli setup` or `uv tool install google-agents-cli`)
4. *(Optional)* **Ollama**: For running local models offline (`brew install ollama` or [ollama.com](https://ollama.com))

---

## ⚡ Quick Start

```bash
# 1. Clone the repository
git clone git@github.com:whatevergeek/agentplayground.git
cd agentplayground/demo

# 2. Configure environment (copy template)
cp .env.example .env
# If using Gemini API, add your key to .env:
# GEMINI_API_KEY="your-api-key-here"

# 3. Install dependencies via uv
agents-cli install

# 4. Launch the interactive browser playground
agents-cli playground
```

Open your browser at **`http://127.0.0.1:8080/dev-ui/?app=app`** to test and inspect the agent.

---

## 🔒 Security Note
This repository includes a strict `.gitignore` configuration ensuring no `.env` files, API keys, credentials, or `.venv/` directories are ever committed. Always use `.env.example` templates when sharing code.

---

## 📜 License
Apache-2.0
