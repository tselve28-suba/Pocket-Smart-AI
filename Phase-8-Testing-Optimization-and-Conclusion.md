# Phase 8 — Testing, Optimization & Conclusion

> Phase 8 of 8 · [Index](README.md) · ← [Phase 7](Phase-7-UI-Development.md)

## 1. Objective of this phase
Verify the system behaves correctly, document known limitations honestly, and define how to deploy and improve it.

## 2. Test plan
| # | Area | What is checked | Method |
|---|---|---|---|
| T1 | Database | Tables are created; users and history are stored | Automated script |
| T2 | Auth | Correct password accepted, wrong password rejected, duplicate username rejected, JWT issued | Automated script |
| T3 | AI engine (fallback) | Each planner returns a valid structure, totals are correct, links are generated | Automated script |
| T4 | Data isolation | A user cannot read another user's history record | Automated script |
| T5 | Live Gemini | Valid JSON, budget adherence, platform accuracy | **Manual** (needs your API key) |
| T6 | End-to-end UI | Forms → results → history in a browser | **Manual** |
| T7 | Edge cases | Budget below minimum, 0 guests, oversized or non-image upload, short password | **Manual** (rules in Phase 6) |

## 3. Automated results (T1–T4)
Run against the project's real `db.py`, `auth.py` and `gemini_utils.py` with no API key set, so the fallback path was exercised. Full log: [`test-output.txt`](test-output.txt).

| Check | Result |
|---|---|
| Create user | ✅ `True` |
| Duplicate username | ✅ rejected (`False`) |
| Login with correct / wrong password | ✅ accepted / ✅ rejected (`None`) |
| JWT has 3 segments | ✅ |
| Home fallback (₹50,000) | ✅ 5 items, total ₹19,500, within budget |
| Party fallback (₹30,000, 40 guests) | ✅ 4 items, total ₹29,100, within budget |
| Jewelry fallback (₹5,000) | ✅ 4 items, total ₹4,750, within budget |
| History save, read own record | ✅ |
| History read as a different user | ✅ blocked (`None`) |

> **Test scope disclosure:** these tests did not call the real Gemini API or send HTTP requests through FastAPI (no API key or network in the test environment). In `auth.py`, the `fastapi.Request` import was stubbed so the logic could run. T5–T7 should be run on your machine before release.

## 4. Manual test checklist (T5–T7)
- [ ] Register → login → dashboard shows your username
- [ ] Home planner with 2 rooms returns items from IKEA / Amazon / Flipkart; badge says **Gemini AI**
- [ ] Party planner (e.g. ₹30,000, 40 guests, Birthday) returns catering, decoration, entertainment
- [ ] Jewelry planner with and without an outfit image
- [ ] Remove `GEMINI_API_KEY`, restart → badge says **Fallback (offline defaults)**
- [ ] Budget ₹500 in home planner → error message shown
- [ ] Upload a 6 MB image or a PDF in jewelry planner → image ignored, plan still generated
- [ ] History lists every request; **View** reopens the saved plan
- [ ] Log out → `/dashboard` redirects to `/login`

## 5. Optimizations already in the code
- JSON-only response mode plus fence-stripping for reliable parsing
- Totals recomputed server-side instead of trusting the model
- Fallback plans for every planner (no blank screens)
- Parameterized SQL and salted, slow password hashing
- Per-user access check on saved results

## 6. Known limitations (found while reviewing the code)
| Issue | Impact | Suggested fix |
|---|---|---|
| Prices come from the AI, not live platform data | Prices are estimates and may not match the store | Integrate official product/affiliate APIs |
| Product links are search links | User still has to pick the product | Store real product URLs when an API is added |
| Zomato has no link template | Zomato items open a Google search instead | Add a Zomato template to `LINKS` |
| Home fallback only scales **down** | With a large budget the fallback uses little of it (₹19,500 of ₹50,000 in the test) | Scale up toward ~90% of budget as well |
| `over_budget` is flagged, not corrected | A model that exceeds budget still gets shown, with a warning | Re-prompt once, or trim the lowest-priority items |
| `allow_origins=["*"]` CORS | Too open for production | Restrict to your domain |
| Default `SECRET_KEY` fallback | Tokens forgeable if the env var is missing | Fail at startup if not set |
| Cookie has no `secure` flag; no CSRF protection | Weaker security over HTTP | Set `secure=True` behind HTTPS; add CSRF tokens |
| `@app.on_event("startup")` | Deprecated in current FastAPI | Move to a lifespan handler |
| Unpinned dependencies | Future releases may break the app | Pin versions in `requirements.txt` |
| No rate limiting | Anyone with an account can burn API quota | Add per-user limits |

## 7. Deployment guide
1. Push the repo to GitHub (never commit `.env` or `pocketsmart.db`)
2. On the host (Render, Railway, a VM, etc.) set environment variables: `GEMINI_API_KEY`, `GEMINI_MODEL`, `SECRET_KEY`
3. Install: `pip install -r requirements.txt`
4. Start: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Use HTTPS, and a persistent disk or a managed database, because SQLite files on ephemeral hosts are lost on redeploy

## 8. Future enhancements
- Real product search through platform or affiliate APIs
- Budget "re-balance" slider and side-by-side plan comparison
- Save favourites, export a plan as PDF
- More planners (travel, electronics, wardrobe)
- Regional language support

---
## 9. Conclusion
PocketSmart AI shows how generative AI can make everyday budgeting practical. FastAPI provides a small, modular backend; Gemini turns budgets, preferences and even outfit images into structured recommendations; and a fallback layer keeps the product usable when the AI is not. The three planners (home, party, jewelry), secure accounts and saved history make a complete, extensible foundation. The main next step is replacing AI-estimated prices and search links with live platform data.

## 10. Output of Phase 8
**Result:** a verified core (database, auth, engine, data isolation), a manual checklist for the live parts, a risk list with fixes, a deployment guide and a project conclusion.

**Final product — Recommendation History:**

![History page](images/history-page.png)
