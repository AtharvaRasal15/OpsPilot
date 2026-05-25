# OpsPilot

a business operations tool i built to automate stuff like support tickets, invoices, and task routing using AI.

---

## what it does

basically you describe a task or paste invoice details and the AI figures out what type it is and routes it to the right team. it can handle:

- support tickets (login issues, bugs, payment problems etc)
- invoice processing (extracts vendor name, amount, due date from plain text)
- general tasks

there's a console dashboard for the team (admins/operators) and a separate customer-facing page where customers can submit requests.

---

## tech stack

**backend**
- FastAPI
- SQLAlchemy + PostgreSQL
- LangGraph + LangChain for the AI agent
- OpenAI (gpt-4o)
- JWT auth with passlib/bcrypt

**frontend**
- plain HTML/CSS/JS (no framework, kept it simple)
- Inter font from Google Fonts

---

## project structure

```
OpsPilot/
├── backend/
│   ├── main.py           # fastapi app entry point
│   ├── models.py         # database models
│   ├── schemas.py        # pydantic schemas
│   ├── database.py       # db connection
│   ├── auth.py           # jwt helper functions
│   ├── config.py         # env config
│   ├── routes/
│   │   ├── auth_routes.py
│   │   ├── tasks.py
│   │   ├── tickets.py
│   │   └── invoices.py
│   └── agent/
│       ├── graph.py      # langgraph agent graph
│       ├── llm.py        # openai setup
│       └── tools.py      # classify + extract functions
└── frontend/
    ├── index.html        # main console (login + dashboard)
    ├── customer.html     # customer request form
    ├── track.html        # ticket tracking page
    ├── app.js
    ├── customer.js
    ├── track.js
    └── styles.css
```

---

## setup

**1. clone the repo**

```bash
git clone <repo-url>
cd OpsPilot
```

**2. backend setup**

```bash
cd backend
pip install -r requirements.txt
```

create a `.env` file in the backend folder:

```
DATABASE_URL=postgresql://user:password@localhost/opspilot
SECRET_KEY=your_secret_key_here
OPENAI_API_KEY=your_openai_key_here
```

run the server:

```bash
uvicorn main:app --reload
```

api will be at `http://localhost:8000`

**3. frontend**

just open `frontend/index.html` in a browser or serve it with any static server. if you're running the backend locally make sure CORS is allowing your origin (already set up for localhost).

---

## features

- AI classifies tasks into tickets / invoices / general automatically
- ticket auto-routing to billing, support, or engineering team based on content
- invoice data extraction (vendor, amount, due date) from plain text
- admin dashboard with live analytics, kanban view, activity stream
- daily AI summary
- CSV export
- ticket detail panel with status updates, internal notes, timeline
- multi-channel intake (website form, email, whatsapp, telegram, manual)
- customer portal for submitting and tracking requests
- light/dark theme toggle

---

## api endpoints

| method | endpoint | description |
|--------|----------|-------------|
| POST | `/register` | create account |
| POST | `/login` | get JWT token |
| GET | `/tasks` | list all tasks |
| POST | `/tasks` | create + run a task through agent |
| GET | `/tickets` | list tickets |
| GET | `/invoices` | list invoices |
| GET | `/health` | health check |

---

## notes

- the AI agent uses keyword matching first and then falls back to the LLM for complex cases
- currently only supports english
- tested on PostgreSQL, might work on SQLite with small changes

---

## todo / known issues

- [ ] email sending is drafted in the UI but not wired to an actual mail service yet
- [ ] no file upload support for invoices (only text input right now)
- [ ] mobile UI needs work
- [ ] add proper error messages on the frontend

---

made this as a learning project to get into agentic AI and FastAPI. still a work in progress.
