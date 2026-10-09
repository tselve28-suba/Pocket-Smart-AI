# Phase 1 — Project Overview & Problem Definition

> **PocketSmart AI: Your Smart Budget & Recommendation Assistant**
> Phase 1 of 8 · [Index](README.md) · Next → [Phase 2: Architecture & Technology Stack](Phase-2-Architecture-and-Tech-Stack.md)

## 1. Objective of this phase
Define the problem PocketSmart AI solves, who it is for, and what "done" looks like, before any code is written.

## 2. Problem statement
Planning a budget across everyday life needs (decorating a home, hosting a party, choosing jewelry) is overwhelming. Products are spread across many platforms, prices vary widely, and it is hard to know whether a plan really fits the budget.

## 3. Proposed solution
PocketSmart AI is a **GenAI-powered, budget-aware recommendation web app**. The user enters a budget and some preferences; a Google Gemini model returns a structured shopping/planning list whose prices are meant to add up to no more than the budget. Each item links to a search on a relevant platform (Amazon, Flipkart, IKEA, Swiggy, OYO and others).

## 4. Target users
| User | Need |
|---|---|
| New home owners / renters | Furnish rooms (lights, fans, dining tables) without overspending |
| Event hosts | Split a party budget across catering, decoration, entertainment and venue |
| Shoppers for occasions | Find jewelry that suits an occasion, style and (optionally) an outfit photo |

## 5. Scenarios (the three planners)
### Scenario 1 — Home Interior Planner
User enters total budget, style, and for each room the number of lights, ceiling fans and dining tables. The AI recommends cost-effective products from IKEA, Amazon and Flipkart, balancing function, style and price.

### Scenario 2 — Party Planner
User enters budget, guest count, event type, venue and city. The AI splits the budget across catering, decoration, entertainment (and venue/stay if needed) using options from Swiggy, Zomato, OYO and Amazon, tailored to the event type.

### Scenario 3 — Jewelry Planner
User enters budget, occasion and style, and may upload an outfit image (max 5 MB). The AI uses the image to match colours and neckline and suggests pieces from Amazon and Flipkart.

## 6. Scope
**In scope**
- Three planners with budget validation
- User registration, login, logout, JWT-based sessions
- Per-user recommendation history
- Offline fallback recommendations when the AI is unavailable
- Responsive HTML/CSS interface

**Out of scope (current version)**
- Live price scraping or official platform APIs. Links are *search links*, not verified live prices
- Payments or checkout
- Admin dashboard

## 7. Success criteria
- Every planner returns 5–10 items (fallback may return fewer) with price, platform, reason and a link
- Estimated total is shown against the budget, with an over-budget warning when exceeded
- The app still returns a usable plan when Gemini is not configured or fails
- Users can revisit previous plans from the history page

## 8. Deliverables of the overall project
Source code (FastAPI + Jinja2), this 8-phase documentation, and a demo video.

---
## 9. Output of Phase 1
**Result:** an agreed problem statement, three scenarios, scope boundaries and success criteria.

**Visual reference — what the finished product looks like (landing page):**

![PocketSmart AI landing page](images/landing-page.png)
