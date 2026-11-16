# PLAN.md

Week-by-week plan for Larder (working name). Check items off as they're done. If dates slip, move tasks, don't delete them, and note why in `LOG.md`.

## Key dates

| Date | Milestone |
|---|---|
| Sun Nov 15 | Project set up, Play Console account started, tester recruiting begins |
| **Sat Nov 21** | **MVP build uploaded to Play Console closed testing track** |
| Sun Nov 22 | Closed test live with 15+ testers opted in (14-day clock starts) |
| Thu Nov 26 | Thanksgiving — plan for a light day |
| Sun Dec 6 | 14 days complete → apply for production access |
| **Mon Dec 7** | **Final show and tell in class** |
| **Sun Dec 13, 11:59 PM** | **Artifact due** — portfolio card filled in, link submitted on Canvas |

Also submit the portfolio link with an updated Artifact 5 card at the end of each course week.

## Definition of done (for the course)

- [ ] Genuine five-week attempt with a repo that has a log of plans and changes (`PLAN.md`, `LOG.md`, commit history)
- [ ] App running on real testers' phones via Play Console closed testing
- [ ] Production access applied for (approval is a bonus, not required)
- [ ] Portfolio card: title, link (Play listing, repo, or demo video), short reflection

---

## Week 0 — Setup (Nov 15–17)

- [ ] Install Android Studio; create an Empty Compose Activity project
- [ ] Create GitHub repo; add `.gitignore` (Android template + `.env`, keystores, `local.properties`)
- [ ] Add `CLAUDE.md`, `PLAN.md`, `LOG.md`; first commit
- [ ] Register Google Play Console personal developer account; start identity verification
- [ ] Post in course Discord to recruit testers (aim for 15+ to cover drop-off); offer to test classmates' apps in return
- [ ] Collect testers' Google account emails in a private list (not in the repo)
- [ ] Set up project layout from `CLAUDE.md` (`ui/`, `data/`, `domain/`, `notifications/`, `ai/`, `AppContainer`)
- [ ] Add Room, Navigation Compose, WorkManager, kotlinx.serialization dependencies
- [ ] Bottom navigation shell with four tabs: Pantry, Use Soon, Meals, Settings

## Week 1 — MVP (Nov 16–21) → upload to closed testing

**Pantry**
- [ ] `PantryItem` entity: id, name, category, quantity, unit, location, expiryDate, addedDate
- [ ] DAO + repository with Flow queries
- [ ] Pantry list screen grouped by location
- [ ] Add/edit item screen with date picker; delete with undo snackbar
- [ ] `shelf_life.json` + default expiry suggestion when a category is picked

**Use it soon**
- [ ] Query items expiring within N days (N in Settings, default 3)
- [ ] Use Soon screen sorted by expiry
- [ ] WorkManager daily worker + notification channel; request notification permission (Android 13+)

**Meal suggestions**
- [ ] Define `recipes.json` schema: name, servings, ingredients (name, qty, unit), equipment list, steps, estimated cost
- [ ] Write 30+ budget recipes to start (grow to 75+ by Week 3); tag equipment honestly
- [ ] Equipment settings (stovetop, oven, microwave, air fryer, rice cooker, slow cooker, kettle)
- [ ] `domain/RecipeMatcher`: score = share of ingredients in pantry, bonus for expiring items, exclude recipes needing missing equipment — **with unit tests**
- [ ] Meals screen: ranked list showing "you have X of Y ingredients" and which expiring items it uses
- [ ] Recipe detail screen

**Release prep**
- [ ] App icon (simple placeholder is fine), app name, `versionCode` 1
- [ ] Generate upload signing key (store outside repo, back it up)
- [ ] Draft `docs/privacy-policy.md` and host it (GitHub Pages works)
- [ ] Play Console: app listing basics, content rating, data safety form
- [ ] Upload to internal testing; install on your own phone and smoke-test
- [ ] Promote the same build to **closed testing**; send opt-in link to testers

## Week 2 — Planning and money (Nov 22–28)

- [ ] Confirm 12+ testers are opted in; message anyone who hasn't
- [ ] Add in-app "Send feedback" link (email or form)
- [ ] Weekly plan: pick N meals for the week; `domain/PlanBuilder` suggests a set that maximizes ingredient overlap — **with unit tests**
- [ ] Shopping list: aggregate missing ingredients across the plan, normalize units, check off while shopping
- [ ] "Mark as cooked" → subtracts ingredients from the pantry
- [ ] Prices: per-item price entry; cost per serving and weekly total
- [ ] Weekly budget setting + warning when the plan exceeds it
- [ ] Push update 2 to closed testing; note tester feedback in `LOG.md`

## Week 3 — AI and polish (Nov 29–Dec 6)

**Backend** (`/backend`)
- [ ] Choose platform (Cloudflare Workers, Firebase Functions, or Supabase Edge Functions); record the decision in `LOG.md`
- [ ] `POST /scan-receipt`: accepts an image, calls the AI model with the key from an environment secret, returns structured JSON (name, quantity, unit, category, price)
- [ ] Input validation (size/type limits) and basic rate limiting
- [ ] Never log or store images

**App**
- [ ] "Scan receipt" flow: photo picker/camera → loading state → review screen where the user edits/confirms each item → add to pantry with default expiry dates
- [ ] Handle errors gracefully (offline, timeout, unreadable receipt)
- [ ] Update privacy policy and Play data safety form for receipt uploads
- [ ] Stretch: AI fuzzy matching of free-text pantry items to recipe ingredients

**Polish**
- [ ] Empty states for every screen; first-run onboarding (pick equipment, add a few items)
- [ ] "Money saved" stat: value of expiring items that got used
- [ ] Store listing: short and full description, 4+ screenshots, feature graphic
- [ ] Push update 3 to closed testing
- [ ] **Dec 6:** apply for production access (answer questions using tester feedback from `LOG.md`)

## Demo and wrap-up (Dec 7–13)

- [ ] Demo script (3–5 min): problem → live demo (add item, use-soon, suggestions, receipt scan) → how I directed the agent → what I learned
- [ ] Record a short backup demo video in case of live-demo problems
- [ ] Respond to any Play review feedback
- [ ] Clean up README: what it is, screenshots, how to build, architecture summary
- [ ] Fill in portfolio card and submit on Canvas

## Stretch goals (after the core is solid)

- Shared pantry for roommates (needs accounts + sync, e.g. Firebase or Supabase)
- Potluck/group meal coordinator
- Barcode scanning for adding items
- Larger recipe set from a free recipe API
- Home-screen widget for "use it soon"

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Fewer than 12 testers stay opted in for 14 days | Recruit 15+; send reminders; swap testing with classmates |
| Play identity verification or review is slow | Start the account on day one; apply on Dec 6; a submitted application still counts as a genuine attempt |
| Kotlin/Compose learning curve | Have the agent explain new concepts; keep logic in plain Kotlin `domain/` classes that feel like Java |
| Recipe data takes too long to write | Start with 30 recipes; have the agent draft entries, then review and fix them yourself |
| AI receipt parsing is unreliable | Always show a review/edit screen; manual entry remains the main path |
| Scope creep | Anything not in Weeks 1–3 goes to Stretch until the closed test is live |
