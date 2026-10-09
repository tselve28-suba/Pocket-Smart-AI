# Phase 6 — FastAPI Routes & Integration (`main.py`)

> Phase 6 of 8 · [Index](README.md) · ← [Phase 5](Phase-5-AI-Recommendation-Engine.md) · Next → [Phase 7: UI Development](Phase-7-UI-Development.md)

## 1. Objective of this phase
Connect the UI, authentication, database and AI engine through clean FastAPI routes.

## 2. Application setup
- `FastAPI(title="PocketSmart AI")`
- **CORS** middleware enabled (currently `allow_origins=["*"]`; see hardening notes in Phase 8)
- `/static` mounted from `static/`, templates from `templates/` via `Jinja2Templates`
- `startup` event calls `db.init_db()`
- A `NotAuthenticated` exception handler redirects to `/login` (HTTP 303)
- Helper `page()` renders a template with the current user
- Helper `show_result()` saves the result to history, then renders `results.html`

## 3. Complete route map
| Route | Method | Auth | Purpose |
|---|---|---|---|
| `/` | GET | optional | Landing page |
| `/register` | GET / POST | no | Sign-up page and handler |
| `/login` | GET / POST | no | Login page and handler (sets cookie) |
| `/token` | POST | no | JWT for API clients |
| `/logout` | GET | no | Clear cookie |
| `/session-info` | GET | yes | Current user info |
| `/session-data` | GET | yes | Query counts by category |
| `/dashboard` | GET | yes | Planner shortcuts + 5 most recent results |
| `/home-planner` · `/party-planner` · `/jewelry-planner` | GET | yes | Planner forms |
| `/generate-home` | POST | yes | Home recommendations |
| `/generate-party` | POST | yes | Party recommendations |
| `/generate-jewelry` | POST (multipart) | yes | Jewelry recommendations, optional image |
| `/history` | GET | yes | All past recommendations |
| `/recommendations-details/{hid}` | GET | yes | Re-open one saved result (own records only) |

## 4. Input validation
| Planner | Rules |
|---|---|
| Home | Budget ≥ ₹1,000. Rooms, lights, fans and tables are submitted as parallel lists and zipped into room objects |
| Party | Budget ≥ ₹1,000 and guests ≥ 1. Defaults: Birthday, Home, Chennai |
| Jewelry | Budget ≥ ₹500. Image is used only if it is non-empty, **≤ 5,000,000 bytes** and its content type starts with `image/` |
| Register | Password ≥ 6 characters; unique username |

Invalid input re-renders the form with an error message instead of calling the AI.

## 5. Data flow for a generate request
1. Validate the form and verify the JWT cookie
2. Call the matching function in `gemini_utils.py`
3. Save `(user_id, category, budget, inputs, result)` to `history`
4. Render `results.html` with the summary, budget stats, item table and tips

## 6. Running the app
```bash
python main.py
# → http://127.0.0.1:8000        (Swagger UI at /docs)
```
`main.py` starts Uvicorn with `reload=True` on port 8000.

---
## 7. Output of Phase 6
**Result:** a fully wired backend. Every page and API in the route map above is served by `main.py`.

**Visual reference — dashboard after login (planners + recent activity):**

![Dashboard](images/dashboard.png)

![Recent activity](images/recent-activity.png)
