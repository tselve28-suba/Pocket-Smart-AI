# Phase 7 — UI Development

> Phase 7 of 8 · [Index](README.md) · ← [Phase 6](Phase-6-FastAPI-Routes-and-Integration.md) · Next → [Phase 8: Testing, Optimization & Conclusion](Phase-8-Testing-Optimization-and-Conclusion.md)

## 1. Objective of this phase
Build a lightweight, responsive interface that lets users enter budgets, see structured AI results and manage their history.

## 2. Approach
- **Jinja2 templates** extending one `base.html` (navigation, footer, stylesheet)
- Single `style.css` using CSS variables (brand purple/violet palette), cards, grid layout, tables and a mobile breakpoint at 640 px
- Minimal JavaScript: only the "+ Add room" button on the home planner
- Server-side rendering: forms POST to the FastAPI routes from Phase 6

## 3. Templates
| Template | Purpose |
|---|---|
| `base.html` | Navbar (Dashboard / History / Logout when signed in, else Login / Register) and footer |
| `index.html` | Hero, three planner cards, testimonials |
| `register.html` / `login.html` | Auth forms with error and success messages |
| `dashboard.html` | Planner shortcuts and the 5 most recent results |
| `home_planner.html` | Budget, style, dynamic room rows (room, lights, fans, dining tables) |
| `party_planner.html` | Budget, guests, event type, venue, city |
| `jewelry_planner.html` | Budget, occasion, style, optional outfit image (max 5 MB) |
| `results.html` | Source badge, summary, Budget / Estimated total / Remaining stats, item table with **View** links, tips |
| `history.html` | Table of past recommendations with a link to each |

## 4. UX details
- Over-budget plans show a red warning; remaining budget turns red/green accordingly
- A badge on the results page shows whether the plan came from **Gemini AI** or the **Fallback**
- Forms keep native validation (`min`, `required`) in addition to server-side checks
- Item links open in a new tab with `rel="noopener"`

---
## 5. Output of Phase 7
**Result:** every page of the product. The screenshots below come from the project walkthrough document (the reference build); the simplified `templates/` in the repository render the same flow with plainer styling.

### Public pages
![Landing](images/landing-page.png)
![Testimonials](images/testimonials.png)
![Footer](images/footer.png)

### Home Interior Planner
| Input form | Recommendations |
|---|---|
| ![Home form](images/home-planner-form.png) | ![Home results](images/home-recommendations.png) |

### Party Planner
| Input form | Recommendations |
|---|---|
| ![Party form](images/party-planner-form.png) | ![Party results](images/party-recommendations.png) |

### Jewelry Planner (with outfit upload)
| Input form | Recommendations |
|---|---|
| ![Jewelry form](images/jewelry-planner-form.png) | ![Jewelry results](images/jewelry-recommendations.png) |

### History
![History](images/history-page.png)
