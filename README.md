# PocketSmart AI — Project Documentation

**Your Smart Budget & Recommendation Assistant**
A GenAI-powered web app (FastAPI + Google Gemini) that gives budget-aware recommendations for **home interiors**, **party planning** and **jewelry**.

## Documentation in 8 phases
Each phase is a standalone file with an **Output** section at the end.

| Phase | Document | What it covers |
|---|---|---|
| 1 | [Project Overview & Problem Definition](Phase-1-Project-Overview.md) | Problem, users, scenarios, scope, success criteria |
| 2 | [Architecture & Technology Stack](Phase-2-Architecture-and-Tech-Stack.md) | Stack, diagrams, request lifecycle, data model |
| 3 | [Gemini AI Setup & Initialization](Phase-3-Gemini-AI-Setup.md) | API key, model, `.env`, connectivity test |
| 4 | [Data Layer & Authentication](Phase-4-Data-Layer-and-Authentication.md) | SQLite, password hashing, JWT, session routes |
| 5 | [AI Recommendation Engine](Phase-5-AI-Recommendation-Engine.md) | Prompts, JSON contract, fallbacks, links |
| 6 | [FastAPI Routes & Integration](Phase-6-FastAPI-Routes-and-Integration.md) | Route map, validation, data flow |
| 7 | [UI Development](Phase-7-UI-Development.md) | Templates, styling, all screens |
| 8 | [Testing, Optimization & Conclusion](Phase-8-Testing-Optimization-and-Conclusion.md) | Test results, limitations, deployment, conclusion |

## Quick start
```bash
git clone <your-repo-url> && cd Pocket-Smart-AI
python -m venv venv && venv\Scripts\activate     # Linux/macOS: source venv/bin/activate
pip install -r requirements.txt
cp env.example .env                               # add your GEMINI_API_KEY
python main.py                                    # http://127.0.0.1:8000
```
Place HTML files in `templates/` and `style.css` in `static/` (see [Phase 4](Phase-4-Data-Layer-and-Authentication.md)).

## Suggested repo layout
```
Pocket-Smart-AI/
├── README.md
├── main.py  gemini_utils.py  auth.py  db.py
├── requirements.txt  env.example
├── static/  templates/
└── docs/            ← put this folder's contents here
    ├── README.md  Phase-1 … Phase-8 .md
    ├── images/
    └── test-output.txt
```
