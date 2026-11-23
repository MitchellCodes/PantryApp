# CLAUDE.md

Standing instructions for Claude Code in this repository. Read this file, then `PLAN.md`, then the latest entries in `LOG.md` before starting any task.

## Project

**Name:** Pantry

A budget kitchen web app (installable PWA) that works on any phone or computer. One sentence: *know what's in your kitchen, use it before it goes bad, and eat well on a budget with the equipment you actually have.*

Target users: students and anyone cooking on a tight budget, often with limited kitchen equipment (dorm, shared apartment).

Core features, in priority order:
1. **Pantry** — items with quantity, unit, location (fridge/freezer/pantry), and expiry date.
2. **Use it soon** — list of items expiring within N days (shown on app open; optional web push reminders later).
3. **Meal suggestions** — recipes ranked by how much of the pantry they use (expiring items weighted higher), filtered by the user's equipment.
4. **Weekly plan + shopping list** — one-trip meal-prep plan; shopping list of only what's missing.
5. **Costs** — cost per meal, weekly budget.
6. **AI receipt scan** — photo of a receipt → proposed pantry items for the user to confirm.

Stretch goal (only after the PWA is live and stable): a native Android app. Do not start it unprompted.

This is a five-week course project (TCSS 490 Artifact 5) and a portfolio piece. A live, working, well-documented deployment matters more than feature count.

## Stack

### Confirmed

- **Delivery:** installable PWA (web app manifest + service worker, offline-capable) usable on any device; native Android app is a stretch goal only
- **Persistence model:** local-first in the browser; no user accounts in the MVP
- **AI backend:** small serverless function in `/backend` that holds the API key (see Security); platform not yet chosen
- **Language:** TypeScript with `strict` mode on (decided 2026-11-22)
- **UI framework:** React with function components and hooks (decided 2026-11-22; chosen for the largest job market and ecosystem)
- **Developer background:** Java — prefer clear, explicit, typed code over clever idioms, and explain unfamiliar concepts briefly when introducing them

### Not yet decided

Nothing below is chosen. Do not install, scaffold, or write code that depends on any of these until the developer picks one. When a decision comes up, present 2–3 options with trade-offs and a recommendation, then wait. Record each decision in `LOG.md` and move it up to Confirmed.

- **Build tool:** Vite (candidate) vs. Next.js, Parcel
- **Routing:** React Router (candidate) vs. TanStack Router, framework built-in
- **Local storage:** Dexie/IndexedDB (candidate) vs. raw IndexedDB, idb, localForage, SQLite WASM
- **PWA tooling:** `vite-plugin-pwa` (candidate) vs. hand-written service worker, Workbox
- **Testing:** Vitest (candidate) vs. Jest; React Testing Library for components
- **Linting/formatting:** ESLint + Prettier (candidate) vs. Biome
- **Hosting:** Cloudflare Pages (candidate) vs. Vercel, Netlify, GitHub Pages
- **CI/CD:** GitHub Actions (candidate) vs. the host's built-in builds
- **AI backend platform:** Cloudflare Workers (candidate) vs. Firebase Functions, Supabase Edge Functions

Do not add any library without asking first. When you suggest one, say why and what the alternative is.

## Project layout

*Provisional: assumes the candidate stack above. Revise once the open decisions are made.*

```
src/
  features/      # One folder per feature: pantry, use-soon, meals, plan, settings
  components/    # Shared UI components
  data/          # Dexie database, table types, repositories
  domain/        # Pure TypeScript logic: recipe matching, plan generation, cost math (no React or browser imports)
  api/           # Client for the AI backend
  App.tsx
  main.tsx
public/
  recipes.json      # Curated bundled recipes
  shelf_life.json   # Default shelf-life days by item category
  manifest.webmanifest, icons/
backend/            # Serverless function for AI calls
docs/               # Privacy policy, screenshots
```

Keep business logic in `domain/` so it can be unit-tested without a browser.

## Commands

*Provisional: exact scripts depend on the tooling decisions above and will be confirmed once the project is scaffolded.*

- Dev server: `npm run dev`
- Production build: `npm run build`
- Unit tests: `npm test`
- Lint: `npm run lint`
- Type check: `npm run typecheck`

Run tests, lint, and type check before saying a task is done. If something can't be verified (for example, needs a real phone or a deployed environment), say so explicitly.

## Conventions

- One feature per branch; small, focused commits with messages like `pantry: add expiry date picker`.
- Every `domain/` function gets unit tests. Recipe matching and cost math must be tested with edge cases (empty pantry, unit mismatches, zero-cost items).
- React: presentational components take props and callbacks; data access and state live in hooks/repositories, not inline in components.
- User-facing strings are kept together per feature (or in a strings module), not scattered through JSX.
- **Storage must be swappable.** UI and `domain/` never call the database directly. All reads and writes go through repository interfaces defined in `data/` (e.g. `PantryRepository`), with local browser storage as the first implementation. Repository methods should be async and return plain data objects, so a cloud-synced implementation (accounts + sync is a stretch goal) can replace the local one without changes elsewhere. Don't leak storage-specific types out of `data/`.
- Local schema changes require a new version with an upgrade function (never delete or reset user data once testers have the app).
- Dates: store as ISO `YYYY-MM-DD` strings (date-only, no time zones); parse and compare in `domain/`.
- Units: normalize quantities into a small fixed set (g, ml, count) in `domain/` before comparing.
- Mobile first: design for a phone-width screen, then scale up. Check keyboard and screen-reader basics.

## Security and privacy

- **Never put an API key, token, or secret in the frontend, in source, or in committed config.** The app talks only to our backend; the backend holds the key as an environment secret.
- `.env*` files and `.dev.vars` stay in `.gitignore`.
- Receipt images are sent to the backend only when the user taps "Scan receipt"; do not log image contents or store them server-side. Keep the privacy policy in `docs/privacy-policy.md` accurate whenever data handling changes.
- The backend must validate input size/type, restrict allowed origins (CORS), and rate-limit requests.
- Treat text extracted from receipts or returned by the AI model as untrusted data: validate and show it to the user for confirmation before writing to the pantry. Never render it as raw HTML.

## Working style

- **Do not build or scaffold anything unless the developer asks.** Reading and summarizing are fine without asking.
- **Implement one class/module/file at a time** and stop so the developer can verify the output before moving to the next.
- Before writing code for a non-trivial task, briefly state the plan and which files you'll touch.
- Prefer finishing one vertical slice (data → domain → UI → test) over scaffolding many half-built features, but still one file at a time within the slice.
- If a task in `PLAN.md` turns out to be bigger than expected, stop and propose how to split it.
- Ask before: adding dependencies, changing the database schema, changing architecture, or deleting files.
- At the end of each session, append an entry to `LOG.md` (see the template there) and check off completed items in `PLAN.md`.

## Release context

- Distribution: a public web deployment (PWA) with a live URL, auto-deployed from `main` via CI. No app store and no tester minimums.
- Testers are anyone with the link; feedback goes through an in-app "Send feedback" link.
- Keep `main` deployable at all times; use branch preview deploys to check changes before merging.
- Android app (stretch): only after the PWA is stable. A native build or a PWA wrapper (e.g. Trusted Web Activity) would be decided then and recorded in `LOG.md`.
