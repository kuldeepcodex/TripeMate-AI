# ✈️ TripMate AI — A Multi-Agent Travel Planner with MCP

An open-source AI travel planner that turns a natural-language trip request into a practical travel plan with flight suggestions, hotel ideas, weather details, and a day-by-day itinerary.

The project uses a multi-agent workflow built with **LangGraph, LangChain, FastAPI, and MCP tooling**.

---

## 🚀 Why This Project?

Planning a trip usually means jumping between multiple websites, tools, and spreadsheets.

TripMate AI brings this flow into one experience by combining:

- ✈️ Flight research agent
- 🏨 Hotel research agent
- 🌤️ Weather agent
- 💰 Budget analysis agent
- 🗺️ Itinerary planning agent
- 🧠 Supervisor agent
- 👤 Human-in-the-loop approval
- 🤖 Final response agent

These agents are coordinated through a **LangGraph workflow** with **MCP-based tool integrations**.

---

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/23749278-8bfe-4465-9e00-cf79abe9f1bc" />


## ✨ Features

- ✈️ Flight research using AviationStack
- 🏨 Hotel suggestions using Tavily Search
- 🌤️ Weather lookup using a custom MCP server
- 🧠 Multi-agent orchestration with LangGraph
- 🔌 MCP-based tool integration
- 💰 Budget feasibility analysis
- 📝 Structured day-by-day itinerary generation
- 👤 Human approval and revision workflow
- 🌐 FastAPI backend with a web interface
- 💾 PostgreSQL-based workflow state persistence
- ⚡ LLM-powered responses using Groq

---

## 🏗️ Architecture

```mermaid
flowchart LR

    A[User] --> B[Web UI]
    B --> C[FastAPI]

    C --> D[LangGraph]

    D --> E[Supervisor]

    E --> F[Flight Agent]
    E --> G[Hotel Agent]
    E --> H[Weather Agent]
    E --> I[Budget Agent]

    F --> J[MCP Tools]
    G --> J
    H --> J

    F --> K[Shared State]
    G --> K
    H --> K
    I --> K

    K --> L[Itinerary Agent]

    L --> M[Human Approval]

    M -->|Approve| N[Final Agent]
    M -->|Revise| L

    N --> O[Final Response]

    D <--> P[(PostgreSQL)]
```

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python 3.10+ | Backend development |
| FastAPI | REST API and web server |
| LangGraph | Multi-agent workflow orchestration |
| LangChain | LLM and AI application components |
| Groq | LLM inference |
| MCP | External tool integration |
| PostgreSQL | LangGraph checkpoint persistence |
| Tavily | Web and hotel search |
| AviationStack | Airport and airline information |
| OpenWeather | Weather and forecast data |
| Jinja2 | HTML templating |
| HTML/CSS/JavaScript | Frontend |
| Docker | Containerization |

---

## 🔌 MCP Integration

TripMate uses **Model Context Protocol (MCP)** to connect agents with external tools.

### MCP Services

- **Tavily MCP**
  - Hotel search
  - Travel information
  - Web search

- **AviationStack MCP**
  - Airport information
  - Airline information
  - Flight-related data

- **Custom Weather MCP Server**
  - Current weather
  - Weather forecast
  - Uses OpenWeather API

### MCP Client

The MCP integration is handled by:

```text
mcp_client.py
```

It provides helper functions such as:

```text
tavily_mcp_search()
aviation_mcp_call()
weather_mcp_search()
forecast_mcp_search()
extract_destination()
```

The main workflow in `backend.py` uses these helpers from the relevant agents.

---

## 🔄 Workflow

```text
User Request
     │
     ▼
   Web UI
     │
     ▼
   FastAPI
     │
     ▼
 LangGraph
     │
     ▼
 Supervisor
     │
     ├──────────────┬──────────────┬──────────────┐
     ▼              ▼              ▼              ▼
 Flight          Hotel          Weather        Budget
 Agent           Agent           Agent           Agent
     │              │              │              │
     └──────────────┴──────────────┴──────────────┘
                            │
                            ▼
                    Itinerary Agent
                            │
                            ▼
                    Human Approval
                       /        \
                      /          \
                 Approve       Revise
                    │             │
                    │             └──────► Itinerary
                    ▼
                Final Agent
                    │
                    ▼
             Final Travel Plan
```

---

## 🧠 How It Works

1. The user submits a natural-language travel request.
2. FastAPI receives the request.
3. LangGraph starts the travel-planning workflow.
4. The supervisor validates the request and selects relevant agents.
5. Specialist agents collect travel information.
6. Agents use MCP tools to access external services.
7. The itinerary agent combines the collected information.
8. The generated itinerary is presented for human approval.
9. The user can approve the itinerary or request revisions.
10. LangGraph resumes the workflow using the persisted state.
11. The final agent generates the polished travel plan.
12. The final response is returned to the web interface.

---

## 📁 Project Structure

```text
TripMate-AI/
│
├── app.py
│   └── FastAPI application entry point
│
├── backend.py
│   └── LangGraph workflow and agents
│
├── mcp_client.py
│   └── MCP client and tool integration
│
├── custom_weather_mcp_server.py
│   └── Custom weather MCP server
│
├── requirements.txt
│   └── Python dependencies
│
├── Dockerfile
│   └── Docker configuration
│
├── static/
│   ├── script.js
│   └── style.css
│
├── templates/
│   └── index.html
│
└── tools/
    └── Flight and web search integrations
```

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Web interface |
| `POST` | `/api/travel` | Start a travel planning request |
| `POST` | `/api/travel/approve` | Approve or revise an itinerary |
| `GET` | `/health` | Health check |

---

## ⚙️ Prerequisites

Before running the project locally, make sure you have:

- Python 3.10+
- PostgreSQL
- Groq API key
- Tavily API key
- AviationStack API key
- OpenWeather API key
- `uvx` for the AviationStack MCP server

---

## 🔐 Environment Variables

Create a `.env` file in the project root:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/travel_db

GROQ_API_KEY=your_groq_api_key

AVIATIONSTACK_API_KEY=your_aviationstack_api_key

TAVILY_API_KEY=your_tavily_api_key

OPENWEATHER_API_KEY=your_openweather_api_key

DEFAULT_ORIGIN_IATA=DAC
```

> Never commit your `.env` file or API keys to GitHub.

---

## ▶️ Running Locally

### 1. Clone the repository

```bash
git clone https://github.com/entbappy/Multi-Agent-System-using-LangGraph-MCP-Supervisor-Guardrails-HITL.git
cd Multi-Agent-System-using-LangGraph-MCP-Supervisor-Guardrails-HITL
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

**macOS/Linux**

```bash
source .venv/bin/activate
```

**Windows**

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure `.env`

Add the required API keys and PostgreSQL connection string.

### 5. Start the application

```bash
python app.py
```

Or, if the application exposes the FastAPI `app` object:

```bash
uvicorn app:app --reload
```

Then open:

```text
http://localhost:8000
```

---

## 💾 State Persistence

TripMate uses PostgreSQL with LangGraph's checkpointer to persist workflow state.

This allows the system to:

- Store graph checkpoints
- Associate state with a `thread_id`
- Pause during human approval
- Resume the workflow later
- Preserve intermediate agent results

PostgreSQL is used here primarily for **workflow/checkpoint persistence**, not as the main travel-data database.

---

## 👤 Human-in-the-Loop

Before generating the final response, the workflow pauses for human review.

```text
Itinerary Generated
        │
        ▼
 Human Approval
    /        \
Approve      Revise
   │           │
   ▼           ▼
Final       Feedback
Agent          │
               └──────► Itinerary
```

This is implemented using LangGraph's interruption/resume capabilities.

---

## 🐳 Docker

The application can be containerized using Docker.

```bash
docker build -t tripmate-ai .
```

Run:

```bash
docker run -p 8000:8000 --env-file .env tripmate-ai
```

---

## 🔮 Future Improvements

- ✈️ Real flight booking integration
- 🏨 Hotel booking integration
- 🚆 Train and local transportation agents
- 💳 Live currency conversion
- 🗺️ Map integration
- 🔐 Authentication and user accounts
- ⚡ Parallel execution of independent agents
- 📊 LangSmith observability
- 🧪 Automated agent evaluation
- 📱 Mobile application

---

## 📄 License

This project is licensed under the Apache 2.0 License.
