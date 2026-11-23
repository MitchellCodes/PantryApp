# PantryApp

**Know what's in your kitchen, use it before it goes bad, and eat well on a budget with the equipment you actually have.**

Pantry is an installable web app (PWA) for students and anyone cooking on a tight budget. It runs in the browser on any phone or computer, and can be added to your home screen. It tracks your pantry and expiry dates, shows what to use soon, suggests meals you can make from what you already have with the equipment you own, and plans a one-trip weekly shop.

> 🚧 **Status:** In active development (November–December 2026). Not deployed yet. See [PLAN.md](PLAN.md) for progress.

---

## Features

| Feature | Status |
|---|---|
| **Pantry** — track items, quantities, location (fridge/freezer/pantry), and expiry dates | Planned · Phase 1 |
| **Use it soon** — list of items about to expire | Planned · Phase 1 |
| **Meal suggestions** — recipes ranked by what's in your pantry, prioritizing expiring items, filtered by your equipment | Planned · Phase 2 |
| **Weekly meal-prep plan** — pick meals that share ingredients so one shopping trip covers the week | Planned · Phase 2 |
| **Smart shopping list** — only what's missing from your plan | Planned · Phase 2 |
| **Cost tracking** — cost per serving and a weekly budget | Planned · Phase 2 |
| **Receipt scan (AI)** — photograph a receipt and review the items it finds before adding them to your pantry | Stretch |
| **Native Android app** | Stretch |

Update this table as features ship.

## Screenshots

_Coming soon._

<!-- Add screenshots here, e.g.
<p float="left">
  <img src="docs/screenshots/pantry.png" width="250" />
  <img src="docs/screenshots/meals.png" width="250" />
  <img src="docs/screenshots/use-soon.png" width="250" />
</p>
-->

## Try it

_Live link coming soon._ Once deployed, open the link in any modern browser. On a phone, use "Add to Home Screen" (or "Install app") to run it like a native app. It works offline after the first load.

Feedback is welcome through the in-app **Send feedback** link.

## Tech stack

The delivery model is decided; most tooling is not. Choices are made one at a time and recorded in [LOG.md](LOG.md).

**Decided**
- Installable PWA (web app manifest + service worker), usable on any device
- Local-first: data is stored in the browser, no account required
- AI features go through a small serverless backend that holds the API key, so no secrets ship in the app

**To be decided:** language, UI framework, build tool, local storage library, PWA tooling, testing and linting, hosting and CI/CD, AI backend platform. See "Not yet decided" in [CLAUDE.md](CLAUDE.md).

## Project structure

Provisional, until the stack is chosen:

```
src/
  features/       One folder per feature (pantry, use-soon, meals, plan, settings)
  components/     Shared UI components
  data/           Local database and repositories
  domain/         Pure logic (recipe matching, plan building, cost math)
  api/            Client for the AI backend
public/
  recipes.json    Bundled budget recipes
  shelf_life.json Default shelf-life days by category
backend/          Serverless function for receipt scanning (if built)
docs/             Privacy policy, screenshots
```

Business logic lives in `domain/` with no UI or browser dependencies, so it's covered by fast unit tests.

## Building from source

_Instructions will be added once the project is scaffolded and the stack is chosen._ The expected shape is: clone the repo, install dependencies, and run a dev server, plus commands for tests, lint, and a production build.

```bash
git clone <repo-url>
cd PantryApp
```

### Backend (optional, for receipt scanning)

The app works without the backend. Only the receipt-scan feature needs it.

1. See `backend/README.md` for setup on the chosen platform.
2. Set the AI provider API key as an **environment secret** on the platform. Never commit it.
3. Point the app at the backend URL through an untracked environment file.

## Privacy

- Pantry data is stored **only in your browser, on your device**. Clearing site data removes it.
- If receipt scanning is added, photos are sent to the backend **only when you tap "Scan receipt"**. They are processed to extract items and are not stored.
- Full policy: [docs/privacy-policy.md](docs/privacy-policy.md) _(to be written)_

## How this was built

This app is my final project ("The Moonshot") for **TCSS 490: Generative AI and Software Development** at the University of Washington Tacoma, Autumn 2026. It was built with **Claude Code** as an AI coding agent, and I directed the work through a planned, logged workflow:

- [CLAUDE.md](CLAUDE.md): standing instructions for the agent (project rules, decisions, security)
- [PLAN.md](PLAN.md): phased plan with checkboxes
- [LOG.md](LOG.md): session-by-session log of decisions, agent notes, and tester feedback

## Roadmap

After the course project, possible next steps include a native Android app, AI receipt scanning, a shared pantry for roommates, a potluck/group meal planner, barcode scanning, a larger recipe library, and push reminders. See the Stretch goals section in [PLAN.md](PLAN.md).

## License

_TBD._ Choose one before making the repo public (for example, MIT or Apache 2.0).
