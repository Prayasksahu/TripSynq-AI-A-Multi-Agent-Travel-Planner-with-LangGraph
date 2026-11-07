<h1 align="center">✈️ TripSynq AI</h1>

<p align="center">
  <strong>Multi-Agent Travel Planner powered by LangGraph</strong><br>
  Plan trips with AI-assisted flight research, hotel discovery, and day-by-day itineraries.
</p>

<p align="center">
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/blob/main/LICENSE"><img src="https://img.shields.io/github/license/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph?style=for-the-badge" alt="MIT License"></a>
  <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.13"></a>
  <a href="https://fastapi.tiangolo.com/"><img src="https://img.shields.io/badge/FastAPI-0.154.0-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI"></a>
  <a href="https://docs.langchain.com/oss/python/langgraph/overview"><img src="https://img.shields.io/badge/LangGraph-1.2.16-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" alt="LangGraph"></a>
</p>

<p align="center">
  <a href="https://groq.com/"><img src="https://img.shields.io/badge/LLM-Groq-F55036?style=for-the-badge" alt="Groq"></a>
  <a href="https://www.postgresql.org/"><img src="https://img.shields.io/badge/Database-PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"></a>
  <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"></a>
  <a href="https://tavily.com/"><img src="https://img.shields.io/badge/Search-Tavily-6C47FF?style=for-the-badge" alt="Tavily"></a>
</p>

<p align="center">
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/stargazers"><img src="https://img.shields.io/github/stars/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph?style=social" alt="GitHub stars"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/network/members"><img src="https://img.shields.io/github/forks/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph?style=social" alt="GitHub forks"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/issues"><img src="https://img.shields.io/github/issues/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph?style=flat-square" alt="GitHub issues"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/commits/main"><img src="https://img.shields.io/github/last-commit/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph?style=flat-square" alt="Last commit"></a>
</p>

<p align="center">
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph"><img src="https://img.shields.io/badge/View%20Source-GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="View source on GitHub"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/issues/new"><img src="https://img.shields.io/badge/Report%20a%20Bug-Open%20Issue-D73A4A?style=for-the-badge&logo=github&logoColor=white" alt="Report a bug"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/issues/new?labels=enhancement"><img src="https://img.shields.io/badge/Request%20Feature-Suggest%20an%20Idea-2563EB?style=for-the-badge&logo=github&logoColor=white" alt="Request a feature"></a>
  <a href="https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-2EA44F?style=for-the-badge&logo=github&logoColor=white" alt="Read license"></a>
</p>

---

## Overview

TripSynq AI is an AI-powered travel-planning web application that turns a natural-language travel request into a structured travel plan. It combines flight-status lookup, hotel research, and LLM-generated itinerary planning through a workflow orchestrated with **LangGraph**.

The application provides a simple web interface where users can describe a trip, review the generated plan, copy the result, or download it as a PDF.

## What it does

- **Natural-language trip requests:** Describe a destination, duration, origin, budget, and travel preferences in one prompt.
- **Flight information:** Searches flight-status data through the AviationStack API and resolves many city or country names to airport IATA codes.
- **Hotel discovery:** Uses Tavily web search to find hotel-related information and source links.
- **AI itinerary generation:** Uses a Groq-hosted language model to create a practical, budget-aware itinerary from the request and gathered research.
- **Structured final response:** Produces a response organized around trip summary, flight information, hotel suggestions, a day-by-day itinerary, estimated budget, and recommendations.
- **Conversation continuity:** Reuses a thread ID in the browser and stores LangGraph checkpoints in PostgreSQL.
- **Export and usability:** Includes quick-start prompts, copy-to-clipboard, PDF export, loading/error states, and a responsive interface.

> **Important:** TripSynq AI is a planning and research assistant, not a booking platform. The current flight integration returns flight-status information, not ticket fares. Hotel results are web-search results; they do not confirm room availability, prices, or reservations.

## How the workflow works

The backend is implemented as a **sequential LangGraph state graph**. Each node receives the shared travel state and passes its output to the next node.

```mermaid
flowchart TD
    A[User enters a travel request] --> B[FastAPI endpoint]
    B --> C[LangGraph workflow]
    C --> D[Flight Agent]
    D --> E[Hotel Agent]
    E --> F[Itinerary Agent]
    F --> G[Final Response Agent]
    G --> H[Return travel plan to the UI]
    C <--> I[(PostgreSQL checkpoint storage)]
    D --> J[AviationStack API]
    E --> K[Tavily Search API]
    F --> L[Groq LLM]
    G --> L
```

### Agent responsibilities

| Workflow node | Responsibility | Integration |
|---|---|---|
| `flight_agent` | Searches flight-status data based on the user's route or destination. | AviationStack |
| `hotel_agent` | Searches the web for hotel information and relevant links. | Tavily |
| `itinerary_agent` | Drafts a practical itinerary using the request, flight results, and hotel research. | Groq (`openai/gpt-oss-120b`) |
| `final_agent` | Combines the collected information into the final formatted travel response. | Groq (`openai/gpt-oss-120b`) |

The flight and hotel nodes run one after another in the current implementation; they are not parallel agents.

## Technology stack

| Area | Technologies |
|---|---|
| Backend/API | Python, FastAPI, Uvicorn, Pydantic |
| Agent orchestration | LangGraph, LangChain |
| Language model | Groq — `openai/gpt-oss-120b` |
| Flight data | AviationStack API, `airportsdata`, `pycountry` |
| Web search | Tavily |
| Conversation persistence | PostgreSQL, Psycopg, LangGraph PostgreSQL checkpointer |
| Frontend | HTML, CSS, JavaScript |
| Rendering and export | Marked.js, html2pdf.js |
| Deployment | Docker |

## Project structure

```text
TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph/
│
├── app.py                 # FastAPI application and HTTP endpoints
├── backend.py             # LangGraph state, agents, workflow, and PostgreSQL checkpointer
├── requirements.txt       # Python dependencies
├── Dockerfile             # Container configuration
├── test.py                # Command-line test runner
│
├── tools/
│   ├── flight_tool.py     # AviationStack flight lookup and airport resolution
│   └── tavily_tool.py     # Tavily hotel/web search helper
│
├── templates/
│   └── index.html         # Main web page
│
├── static/
│   ├── style.css          # UI styling and responsive layout
│   └── script.js          # Frontend interactions and API requests
│
├── demo.excalidraw        # Excalidraw project/demo file
├── LICENSE
└── README.md
```

## Getting started

### Prerequisites

- Python **3.13**
- A PostgreSQL database that the application can connect to
- API keys for Groq, Tavily, and AviationStack

### 1. Clone the repository

```bash
git clone https://github.com/Prayasksahu/TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph.git
cd TripSynq-AI-A-Multi-Agent-Travel-Planner-with-LangGraph
```

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
DATABASE_URL=postgresql://username:password@host:5432/database?sslmode=require

# Optional: a valid three-letter IATA airport code, for example DEL
DEFAULT_ORIGIN_IATA=DEL
```

The application reads these variables at startup:

| Variable | Required | Purpose |
|---|---|---|
| `GROQ_API_KEY` | Yes | Authenticates requests to the Groq language model. |
| `TAVILY_API_KEY` | Yes | Authenticates hotel/web searches. |
| `AVIATIONSTACK_API_KEY` | Needed for flight lookups | Authenticates requests to AviationStack. |
| `DATABASE_URL` | Yes | PostgreSQL connection string used by the LangGraph checkpointer. |
| `DEFAULT_ORIGIN_IATA` | Optional | Fallback origin airport code when the prompt contains only a destination. |

**Airport-origin configuration:** Set `DEFAULT_ORIGIN_IATA` to a valid three-letter IATA code such as `DEL`, `BOM`, or `BLR`. The current source-code fallback is `IN`, which is a country code rather than an airport IATA code; setting this variable avoids relying on that fallback for destination-only requests.

The backend adds `sslmode=require` when the database URL does not specify an SSL mode. For a local PostgreSQL instance that does not support SSL, explicitly add `?sslmode=disable` to its connection URL.

Keep `.env` and all API credentials private. Do not commit secrets to GitHub.

### 5. Start the application

```bash
uvicorn app:app --reload
```

Open the application at:

```text
http://127.0.0.1:8000
```

FastAPI's interactive API documentation is available at:

```text
http://127.0.0.1:8000/docs
```

The PostgreSQL checkpointer is initialized by the backend during startup. Make sure the database is reachable and the configured credentials have permission to create the checkpoint tables.

## Example prompts

Try requests such as:

- `Plan a 7-day Japan trip from India with flights, hotels, sightseeing, and a budget of ₹2,00,000.`
- `Plan a 5-day Dubai trip from Mumbai with hotel suggestions and things to do.`
- `Create a 7-day Thailand itinerary with budget-friendly hotel research.`
- `Find live flight information from Delhi to Singapore.`

Results depend on the availability, coverage, and limits of the external APIs, as well as the information included in the prompt.

## API reference

### `GET /`

Serves the TripSynq AI web interface.

### `GET /health`

Returns a basic health response:

```json
{
  "status": "ok",
  "message": "AI Travel Planner API is running"
}
```

### `POST /api/travel`

Generates a travel plan.

**Request body:**

```json
{
  "message": "Plan a 7-day trip to Japan from India under 2 lakhs, including flights and hotels.",
  "thread_id": null
}
```

`message` is required. `thread_id` is optional: omit it or pass `null` to start a new thread, then send the returned thread ID with later requests to continue that thread.

**Successful response fields:**

| Field | Description |
|---|---|
| `success` | Indicates whether the request completed successfully. |
| `thread_id` | Thread identifier used for conversation continuity. |
| `answer` | Final generated travel plan. |
| `flight_results` | Flight lookup output returned by the flight tool. |
| `hotel_results` | Hotel/web search output returned by Tavily. |
| `itinerary` | Itinerary drafted by the itinerary node. |

An empty message returns HTTP `400`. Backend or integration errors return HTTP `500` with an error message.

## Run with Docker

Build the image from the repository root:

```bash
docker build -t tripsynq-ai .
```

Run the container using the environment file created earlier:

```bash
docker run --env-file .env -p 8000:8000 tripsynq-ai
```

Then open `http://localhost:8000`. The PostgreSQL database must be reachable from the container; use a database hostname accessible from the Docker network rather than assuming that `localhost` inside the container refers to your host machine.

## Current limitations and notes

- **No booking flow:** The app does not book flights or hotels, process payments, or reserve rooms.
- **Flight fares:** AviationStack is used for live/status flight information. Ticket pricing is not returned by the current flight tool.
- **Hotel availability:** Tavily provides web-search results, not verified room inventory or real-time hotel availability.
- **Generated estimates:** Itinerary details and budget estimates are LLM-generated suggestions. Verify dates, fares, hotel rates, visa rules, opening hours, and other travel requirements with authoritative providers before making bookings.
- **External dependencies:** Requests can fail or return incomplete data when an API key is missing, a provider is unavailable, a plan's quota is exhausted, or a route is not covered.
- **Thread storage:** Conversation checkpoints are stored in PostgreSQL. The browser stores the active thread ID in `localStorage`; clearing browser storage may lose the browser's reference to an existing thread.

## Future improvements

Potential next steps for the project include:

- Add a dedicated flight-pricing provider for fare comparisons.
- Add hotel availability and booking-provider integrations.
- Improve route, date, passenger, and budget extraction with structured validation.
- Add richer source attribution and data freshness indicators to generated plans.
- Add automated tests for API endpoints, graph nodes, and external-tool error handling.

## License

This project is licensed under the [MIT License](LICENSE).

---

Made with Python, LangGraph, FastAPI, and AI-powered travel research.
