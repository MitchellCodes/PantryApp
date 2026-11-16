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
