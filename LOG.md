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
