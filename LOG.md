# LOG.md

A running log of plans, decisions, and changes. Add a new entry at the **top** at the end of every work session (Claude Code appends one when asked, per `CLAUDE.md`). Check the course's "Keep a log" guidance and adjust this format if it asks for anything extra.

## Entry template

```
## YYYY-MM-DD — <short session title>

**Goal:** what this session set out to do (link the PLAN.md item)
**Done:**
- ...
**Decisions:** choices made and why (libraries, architecture, scope cuts)
**Agent notes:** what worked or didn't when directing Claude Code (prompts, corrections, surprises)
**Tester feedback:** (if any)
**Next:** the first thing to do next session
**Blockers:** (if any)
```

---

## 2026-11-22 — Decision: build tool

**Goal:** Make the third Phase 0 technology decision (`PLAN.md`).
**Done:**
- Compared Vite, Next.js, and Parcel, then a deeper Vite vs. Next.js comparison
- Chose Vite; recorded it as Confirmed in `CLAUDE.md` and ticked it in `PLAN.md`
- Added three framework-isolation rules to `CLAUDE.md` Conventions
**Decisions:**
- **Vite.** It matches the app's shape (client-only, offline-first PWA with browser storage), has the smoothest PWA and Vitest integration, outputs portable static files, and carries the least risk in the short build window. Next.js's strengths (server rendering, API routes, built-in backend) mainly pay off with accounts and a server database.
- **Next.js stays a realistic later option** if the cross-device sync stretch goal is built. Keeping `domain/`, `data/` repositories, and components free of framework imports means a swap rewrites only the thin shell (routing, entry, PWA and build config). Estimated one to two days, plus adding `"use client"` markers.
- **New rules:** framework-specific code stays in a thin shell; framework APIs are wrapped in small helpers; no browser-only work at module load.
**Agent notes:** Interfaces can't abstract the framework itself. Layering and isolation are what keep a swap cheap.
**Next:** Decide local storage.
**Blockers:** None.

---

## 2026-11-22 — Decision: UI framework

**Goal:** Make the second Phase 0 technology decision (`PLAN.md`).
**Done:**
- Compared React, Vue, and Svelte
- Chose React; recorded it as Confirmed in `CLAUDE.md` and ticked it in `PLAN.md`
**Decisions:**
- **React (function components + hooks).** The stated priority was maximizing the chance of getting hired. React has the largest job market and ecosystem, and the most examples and tutorials. React experience also transfers well to Vue (components, props, state, tooling), though it doesn't replace Vue-specific keywords in listings that require Vue. Vue was the gentler alternative; Svelte has the smallest job market.
**Agent notes:** Asked whether React experience helps with Vue-requiring listings before deciding; the answer was yes for concepts and tooling, not for Vue-specific APIs.
**Next:** Decide the build tool.
**Blockers:** None.

---

## 2026-11-22 — Decision: language

**Goal:** Make the first Phase 0 technology decision (`PLAN.md`).
**Done:**
- Compared TypeScript, JavaScript, and Kotlin (Compose Multiplatform for web)
- Chose TypeScript; recorded it as Confirmed in `CLAUDE.md` and ticked it in `PLAN.md`
**Decisions:**
- **TypeScript with `strict` on.** Static types and interfaces feel closest to Java among the web options, it's the industry standard for web apps, and the type checker helps verify generated code one file at a time. Plain JavaScript gives up that safety. Kotlin for the web was rejected: the web target is still maturing, Room doesn't support it, and tooling would eat the short build window.
**Agent notes:** Presented a comparison table with a recommendation and waited for the pick, per the "Not yet decided" rule in `CLAUDE.md`.
**Next:** Decide the UI framework.
**Blockers:** None.

---

## 2026-11-22 — Switch from Android/Play Store to a PWA

**Goal:** Revisit how the app is delivered after the Nov 21 Play closed-testing milestone slipped, and update all planning files to match.
**Done:**
- Reviewed the repo (docs only; no code or project exists yet)
- Compared distribution options (APK/GitHub Releases, Firebase App Distribution, Play internal/closed testing, PWA)
- Rewrote `CLAUDE.md`, `PLAN.md`, and `README.md` for an installable PWA
- Renamed the app from Larder to Pantry and removed the working-name notes
- Split the stack in `CLAUDE.md` into Confirmed vs. Not yet decided
**Decisions:**
- **PWA instead of an Android app on Google Play.** Reasons: the project has no deployment requirement; the Play closed test needs 12+ testers for 14 days, which is the main risk; the goal is "usable anywhere" (including iPhone and desktop); and a live URL with CI/CD is a stronger portfolio piece and closer to how a real company ships a web product. Nothing had been built, so the switch cost nothing.
- **Native Android app is now a stretch goal.** It would bring back Play Console and its tester requirement.
- **Local-first, no accounts for the MVP.** Cross-device sync was considered. It needs accounts, a cloud database, a sync mechanism, and conflict handling, which would cost one to two weeks and conflicts with the Dec 6 feature freeze. It is a stretch goal.
- **Storage must be swappable.** All data access goes through a repository interface so a synced backend can replace local storage later without touching UI or `domain/`.
- **Scope cuts to fit about two weeks of build time:** AI receipt scan is optional (only if Phase 2 finishes early), and "money saved" and web push reminders moved to stretch.
- **Technology choices stay open** (language, UI framework, build tool, local storage, PWA tooling, testing, linting, hosting, CI/CD, AI backend platform). Each will be decided one at a time with options and a recommendation, then recorded here.
**Agent notes:** Rule set for Claude Code: don't build anything unless asked, and implement one class/file at a time so each can be verified. Added to `CLAUDE.md` Working style.
**Tester feedback:** None yet.
**Next:** Make the Phase 0 technology decisions one at a time (starting with language and UI framework), then scaffold the project and get a "hello world" live at a public URL.
**Blockers:** None. Timeline is tight: stack and live deploy targeted for Nov 24, feature freeze Dec 6.

---

## 2026-11-15 — Project kickoff

**Goal:** Choose the Artifact 5 project and set up planning files.
**Done:**
- Chose the budget kitchen app (pantry + expiry tracking + meal suggestions by equipment + weekly meal-prep plan)
- Picked stack: Kotlin + Jetpack Compose, Room, WorkManager; Android only, released on Google Play
- Wrote `CLAUDE.md`, `PLAN.md`, and this log
**Decisions:**
- Android only: I have an Android phone and a Windows/Linux PC; iOS would need a Mac
- Kotlin over Java: official Android stack, close to Java, good learning story
- Local-first, no accounts in the MVP to keep scope small
- AI calls go through a small backend so no API key ships in the app
**Next:** Install Android Studio, create the repo, register the Play Console account, recruit testers.
**Blockers:** None yet.
