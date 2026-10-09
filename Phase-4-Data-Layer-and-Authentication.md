# Phase 4 — Data Layer & Authentication

> Phase 4 of 8 · [Index](README.md) · ← [Phase 3](Phase-3-Gemini-AI-Setup.md) · Next → [Phase 5: AI Recommendation Engine](Phase-5-AI-Recommendation-Engine.md)

## 1. Objective of this phase
Build the storage and security foundation: user accounts, password hashing, JWT sessions and recommendation history.

## 2. Project structure
The source expects this layout (the downloaded zip is flat, so move the files into `templates/` and `static/`):
```
Pocket-Smart-AI/
├── main.py
├── gemini_utils.py
├── auth.py
├── db.py
├── requirements.txt
├── env.example
├── static/
│   └── style.css
└── templates/
    ├── base.html  index.html  login.html  register.html
    ├── dashboard.html  history.html  results.html
    └── home_planner.html  party_planner.html  jewelry_planner.html
```
> `main.py` mounts `static/` and loads `templates/`, so the app will not start until the files are in these folders.

## 3. Database module — `db.py`
SQLite file `pocketsmart.db` is created automatically at startup by `init_db()`.

| Function | Purpose |
|---|---|
| `init_db()` | Creates `users` and `history` tables if missing |
| `create_user(username, email, password_hash)` | Inserts a user. Returns `False` if the username already exists |
| `get_user(username)` | Looks up a user |
| `save_history(user_id, category, budget, inputs, result)` | Stores a request and its result as JSON. Returns the new id |
| `list_history(user_id, limit)` | Newest-first history for a user |
| `get_history(hid, user_id)` | Fetches one record **only if it belongs to that user** |

All queries use parameter placeholders (`?`), which protects against SQL injection.

## 4. Authentication module — `auth.py`
| Concern | Implementation |
|---|---|
| Password storage | PBKDF2-HMAC-SHA256, 100,000 iterations, random 16-byte salt, stored as `salt$hash` |
| Password check | `hmac.compare_digest` (constant-time) |
| Token | JWT, HS256, 8-hour expiry (`TOKEN_HOURS`), signed with `SECRET_KEY` |
| Token transport | HTTP-only cookie `access_token` (browser) or `Authorization: Bearer <token>` (API clients) |
| Protected pages | `get_current_user` raises `NotAuthenticated`, which `main.py` turns into a redirect to `/login` |
| Optional auth | `get_optional_user` lets the landing page show different links for logged-in users |

## 5. Session and account routes (implemented in `main.py`)
| Route | Method | Description |
|---|---|---|
| `/register` | GET, POST | Create an account. Password must be ≥ 6 characters; duplicate usernames are rejected |
| `/login` | GET, POST | Verify credentials, set JWT cookie, redirect to `/dashboard` |
| `/logout` | GET | Delete the cookie, redirect to `/login` |
| `/token` | POST | Returns a JWT for API clients (e.g. Swagger `/docs`) |
| `/session-info` | GET | `{user_id, username, logged_in}` |
| `/session-data` | GET | `{username, total_queries, by_category}` from the user's last 100 queries |

---
## 6. Output of Phase 4
**Result:** user accounts, secure login/logout, session endpoints and a history store.

**Verified by test run** (full log in [Phase 8](Phase-8-Testing-Optimization-and-Conclusion.md)):
```
create_user: True
duplicate user: False
auth ok: True | auth bad: None
jwt parts: 3
history saved id: 1 | fetch own: True | fetch other user: None
```

**Visual reference — Register and Login pages:**

| Register | Login |
|---|---|
| ![Register](images/register-page.png) | ![Login](images/login-page.png) |
