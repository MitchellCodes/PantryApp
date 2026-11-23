# PLAN.md

Plan for Pantry, an installable PWA. Check items off as they're done. If dates slip, move tasks, don't delete them, and note why in `LOG.md`.

Technology choices are listed as **decisions** (see "Not yet decided" in `CLAUDE.md`). Each decision gets 2–3 options from Claude with a recommendation; the developer picks; it's recorded in `LOG.md`. Feature items below are written to be stack-neutral until then.

## Key dates

| Date | Milestone |
|---|---|
| Sun Nov 22 | Switched from Android/Play Store to a PWA; plan rewritten |
| **Tue Nov 24** | **Stack decided, project scaffolded, "hello world" live at a public URL with CI/CD** |
| Thu Nov 26 | Thanksgiving — plan for a light day |
| **Sun Nov 29** | **Pantry + Use soon live; link sent to testers** |
| **Sun Dec 6** | **Feature freeze: Meals, plan, costs live; feedback incorporated** |
| **Mon Dec 7** | **Final show and tell in class** |
| **Sun Dec 13, 11:59 PM** | **Artifact due** — portfolio card filled in, link submitted on Canvas |

Also submit the portfolio link with an updated Artifact 5 card at the end of each course week.

## Definition of done (for the course)

- [ ] Genuine five-week attempt with a repo that has a log of plans and changes (`PLAN.md`, `LOG.md`, commit history)
- [ ] App live at a public URL, auto-deployed from `main`, working on phone and desktop
- [ ] Real testers have used it and their feedback is logged
- [ ] Portfolio card: title, live link, repo link, short reflection

---

## Phase 0 — Decisions and setup (Nov 22–24)

**Housekeeping**
- [ ] Review and approve the rewritten `CLAUDE.md`, `PLAN.md`, `README.md`, `LOG.md`
- [ ] Fix `README.md` (no "closed testing" claim; correct clone path)

**Decisions** (one at a time; options + recommendation, then record in `LOG.md`)
- [ ] Language
- [ ] UI framework
- [ ] Build tool
- [ ] Local storage
- [ ] PWA tooling
- [ ] Testing and linting
- [ ] Hosting and CI/CD
- [ ] AI backend platform (can wait until the AI phase)

**Setup**
- [ ] Create the project; add `.gitignore` (Node template + `.env*`, `.dev.vars`)
- [ ] Set up project layout from `CLAUDE.md` (revised after the decisions)
- [ ] CI: run tests + lint on every push
- [ ] Deploy to the chosen host; confirm a public URL loads on a phone
- [ ] Web manifest + service worker: confirm the app is installable
- [ ] App shell with four tabs: Pantry, Use Soon, Meals, Settings
- [ ] Recruit testers (aim for 5–10 people, any device); collect contacts privately, not in the repo

## Phase 1 — Pantry and Use it soon (Nov 24–29)

**Pantry**
- [ ] `PantryItem` type: id, name, category, quantity, unit, location, expiryDate, addedDate
- [ ] Repository interface (storage-agnostic, async, plain data objects) + local storage implementation with change-observing queries, so storage can be swapped for a synced backend later
- [ ] Pantry list screen grouped by location
- [ ] Add/edit item form with date picker; delete with undo
- [ ] `shelf_life.json` + default expiry suggestion when a category is picked

**Use it soon**
- [ ] `domain/` function: items expiring within N days — **with unit tests**
- [ ] Use Soon screen sorted by expiry
- [ ] Setting for N (default 3)
- [ ] Stretch: optional web push reminder (limited on iPhone; in-app list is the baseline)

**Ship**
- [ ] Privacy policy draft in `docs/privacy-policy.md`, linked from the app
- [ ] In-app "Send feedback" link (email or form)
- [ ] Send the live link to testers; note first feedback in `LOG.md`

## Phase 2 — Meals, plan, and costs (Nov 30–Dec 6)

**Meal suggestions**
- [ ] Define `recipes.json` schema: name, servings, ingredients (name, qty, unit), equipment list, steps, estimated cost
- [ ] Write 30+ budget recipes; tag equipment honestly
- [ ] Equipment settings (stovetop, oven, microwave, air fryer, rice cooker, slow cooker, kettle)
- [ ] `domain/` recipe matcher: score = share of ingredients in pantry, bonus for expiring items, exclude recipes needing missing equipment — **with unit tests**
- [ ] Meals screen: ranked list showing "you have X of Y ingredients" and which expiring items it uses
- [ ] Recipe detail screen

**Plan and shopping**
- [ ] `domain/` plan builder suggesting a set of meals that maximizes ingredient overlap — **with unit tests**
- [ ] Shopping list: aggregate missing ingredients, normalize units, check off while shopping
- [ ] "Mark as cooked" subtracts ingredients from the pantry

**Costs**
- [ ] Per-item price entry; cost per serving and weekly total (**unit tests** for edge cases)
- [ ] Weekly budget setting + warning when the plan exceeds it

**Polish**
- [ ] Empty states for every screen; first-run onboarding (pick equipment, add a few items)
- [ ] Check layout on phone and desktop; basic accessibility pass
- [ ] Incorporate tester feedback; log it in `LOG.md`
- [ ] **Dec 6:** feature freeze

## AI receipt scan (only if Phase 2 finishes early; otherwise stretch)

- [ ] Choose backend platform; record in `LOG.md`
- [ ] Backend endpoint: accepts an image, calls the AI model with the key from an environment secret, returns structured JSON (name, quantity, unit, category, price)
- [ ] Input validation (size/type limits), CORS restricted to the app's origin, basic rate limiting
- [ ] Never log or store images
- [ ] "Scan receipt" flow: photo picker/camera → loading state → review screen where the user edits/confirms each item → add to pantry with default expiry dates
- [ ] Handle errors gracefully (offline, timeout, unreadable receipt)
- [ ] Update privacy policy for receipt uploads

## Demo and wrap-up (Dec 7–13)

- [ ] Demo script (3–5 min): problem → live demo (add item, use-soon, suggestions, plan) → how I directed the agent → what I learned
- [ ] Record a short backup demo video in case of live-demo problems
- [ ] Clean up README: what it is, live link, screenshots, how to build, architecture summary
- [ ] Fill in portfolio card and submit on Canvas

## Stretch goals (after the core is solid)

- Accounts and cross-device sync: sign-in, cloud database, offline-first sync, conflict handling, privacy policy update, import of existing local data (swap in a synced implementation of the repository interfaces)
- Native Android app (or a PWA wrapper such as a Trusted Web Activity); would reintroduce Play Console and its tester requirements
- AI receipt scan (if not done above)
- "Money saved" stat: value of expiring items that got used
- AI fuzzy matching of free-text pantry items to recipe ingredients
- Shared pantry for roommates (needs accounts + sync)
- Potluck/group meal coordinator
- Barcode scanning for adding items
- Larger recipe set from a free recipe API
- Web push "use it soon" reminders

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Only about two weeks of build time before Dec 7 | Feature freeze on Dec 6; receipt scan moved to optional; decisions made quickly in Phase 0 |
| Learning a new language/framework (Java background) | Have the agent explain new concepts; keep logic in plain `domain/` modules that feel like Java |
| Web notifications unreliable, especially on iPhone | In-app "use soon" list is the baseline; push is a stretch |
| Browser storage can be cleared | Tell users it's stored on their device; consider export/import as a later feature |
| Recipe data takes too long to write | Start with 30 recipes; have the agent draft entries, then review and fix them yourself |
| AI receipt parsing is unreliable | Always show a review/edit screen; manual entry remains the main path |
| Fewer testers than hoped | Anyone with the link can test; swap testing with classmates |
| Scope creep | Anything outside Phases 0–2 goes to Stretch |
