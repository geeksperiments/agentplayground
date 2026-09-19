# Google ADK Demo Agent (`demo`)

A modular, production-ready AI Agent built using **Google ADK (Agent Development Kit)** and **`google/agents-cli`**. 

This demo features built-in tools (function calling) and is pre-configured to run interchangeably across **Cloud Gemini**, **Local Ollama (Gemma4)**, and **Local Antigravity CLI (`agy-bridge`)**.

---

## 🚀 Key Features

* **Multi-Backend Flexibility**:
  * 🌐 **Google Gemini API**: Direct cloud inference via `google-genai` and `GEMINI_API_KEY`.
  * 💻 **Local Ollama**: 100% offline, on-device execution on Apple Silicon via `gemma4:12b-mlx` or `qwen2.5-coder:14b`.
  * 🌉 **Antigravity CLI Proxy (`agy-bridge`)**: Route queries through your local authenticated `agy` session with zero API key or token fees.
* **Built-in Tools (Function Calling)**:
  * `get_weather(query)`: Simulated location weather lookup.
  * `get_current_time(query)`: City timezone resolution and current timestamp.
* **Interactive Development**:
  * **ADK Web Playground**: Visual browser interface with thought traces, tool execution cards, and multi-turn state.
  * **CLI Runner**: Non-interactive or persistent background server mode via `agents-cli run`.

---

## 📁 Project Structure

```text
demo/
├── app/
│   ├── agent.py               # Main agent definition, model config, and tools
│   ├── fast_api_app.py        # FastAPI server running ADK SSE endpoints
│   └── app_utils/             # A2A, services, and runtime utilities
├── tests/                     # Unit, integration, and evaluation test suites
├── agents-cli-manifest.yaml   # Project lifecycle manifest
├── pyproject.toml             # uv / pip dependency configuration
├── uv.lock                    # Deterministic package lockfile
├── GEMINI.md                  # Context guide for coding assistants
├── Dockerfile                 # Container packaging for deployment
└── README.md                  # Project documentation
```

---

## ⚡ Quick Start

### 1. Install Dependencies
Make sure you have `uv` installed, then run:
```bash
agents-cli install
```
*(Installs all dependencies into `.venv` in sub-second time via uv).*

### 2. Launch the Web Playground
```bash
agents-cli playground
```
Open your browser at:
👉 **`http://127.0.0.1:8080/dev-ui/?app=app`**

### 3. Run in the Terminal
```bash
# Single execution
agents-cli run "What is the weather in San Francisco?"

# Start background server for fast warm iterations
agents-cli run --start-server "Hello!"
agents-cli run "What time is it in SF?"
agents-cli run --stop-server
```

---

## 🔄 Switching Model Backends

To switch how the agent thinks, update the `model` definition in [`app/agent.py`](file:///app/agent.py):

### Option A: Local `agy-bridge` (Current Default)
Routes through your running `agy-bridge` on `http://127.0.0.1:8000/v1` (uses your authenticated `agy` CLI):
```python
from google.adk.models.lite_llm import LiteLlm

model = LiteLlm(
    model="openai/agy-gemini",
    api_base="http://127.0.0.1:8000/v1",
    api_key="none",
)

root_agent = Agent(name="demo", model=model, tools=[get_weather, get_current_time])
```

### Option B: Local Ollama (`gemma4:12b-mlx`)
100% local, runs on Apple Silicon GPU without network:
```python
MODEL = "ollama_chat/gemma4:12b-mlx"

root_agent = Agent(name="demo", model=MODEL, tools=[get_weather, get_current_time])
```

### Option C: Cloud Google Gemini
Direct inference with Google AI Studio key:
```python
from google.adk.models import Gemini
from google.genai import types

MODEL = "gemini-3.7-flash"

root_agent = Agent(
    name="demo",
    model=Gemini(model=MODEL, retry_options=types.HttpRetryOptions(attempts=3)),
    tools=[get_weather, get_current_time],
)
```

---

## 🛠️ Useful CLI Commands

| Command | Purpose |
| :--- | :--- |
| `agents-cli install` | Sync dependencies via `uv` |
| `agents-cli playground` | Start interactive browser UI (auto-reloads on file edits) |
| `agents-cli run "<prompt>"` | Run prompt through terminal |
| `agents-cli eval run` | Run automated evaluation benchmarks against datasets |
| `agents-cli lint` | Lint codebase with ruff |
| `agents-cli scaffold enhance` | Generate deployment manifests and CI/CD pipelines |

---

## 📜 License
Apache-2.0
