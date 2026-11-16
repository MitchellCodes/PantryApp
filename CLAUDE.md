# CLAUDE.md

Standing instructions for Claude Code in this repository. Read this file, then `PLAN.md`, then the latest entries in `LOG.md` before starting any task.

## Project

**Working name:** Larder (placeholder; rename later in `settings.gradle.kts`, `strings.xml`, and the package name)

A budget kitchen app for Android. One sentence: *know what's in your kitchen, use it before it goes bad, and eat well on a budget with the equipment you actually have.*

Target users: students and anyone cooking on a tight budget, often with limited kitchen equipment (dorm, shared apartment).

Core features, in priority order:
1. **Pantry** — items with quantity, unit, location (fridge/freezer/pantry), and expiry date.
2. **Use it soon** — list and daily notification for items expiring within N days.
3. **Meal suggestions** — recipes ranked by how much of the pantry they use (expiring items weighted higher), filtered by the user's equipment.
4. **Weekly plan + shopping list** — one-trip meal-prep plan; shopping list of only what's missing.
5. **Costs** — cost per meal, weekly budget.
6. **AI receipt scan** — photo of a receipt → proposed pantry items for the user to confirm.

This is a five-week course project (TCSS 490 Artifact 5). Shipping a working, released app with real testers matters more than feature count.

## Stack

- **Language:** Kotlin (the developer's background is Java — prefer clear, explicit code over clever Kotlin idioms, and explain unfamiliar Kotlin/Compose concepts briefly when introducing them)
- **UI:** Jetpack Compose, Material 3, Navigation Compose
- **Architecture:** MVVM — Compose screens → ViewModels (StateFlow) → Repositories → Room DAOs
- **Async:** Kotlin coroutines and Flow
- **Persistence:** Room (local-first; no user accounts in the MVP)
- **Background work:** WorkManager (daily expiry check + notification)
- **Serialization:** kotlinx.serialization (bundled recipe data, backend requests)
- **Networking:** Retrofit + OkHttp (only for the AI backend)
- **DI:** manual dependency injection via an `AppContainer` class — do not add Hilt/Dagger unless asked
- **Min SDK:** 26 · **Target SDK:** latest stable
- **AI backend:** small serverless function in `/backend` that holds the API key (see Security)

Do not add new libraries without asking first. When you do suggest one, say why and what the alternative is.

## Project layout

```
app/src/main/java/<package>/
  ui/            # Compose screens, one subpackage per feature (pantry, suggestions, plan, settings)
  ui/theme/      # Material theme, colors, typography
  data/          # Room database, entities, DAOs, repositories
  domain/        # Pure Kotlin logic: recipe matching, plan generation, cost math (no Android imports)
  notifications/ # WorkManager workers, notification channels
  ai/            # Retrofit client for the backend
  AppContainer.kt
app/src/main/assets/
  recipes.json      # Curated bundled recipes
  shelf_life.json   # Default shelf-life days by item category
backend/            # Serverless function for AI calls
```

Keep business logic in `domain/` so it can be unit-tested without an emulator.

## Commands

Use `./gradlew` on Linux/macOS, `gradlew.bat` on Windows.

- Build debug APK: `./gradlew assembleDebug`
- Unit tests: `./gradlew test`
- Instrumented tests (needs emulator/device): `./gradlew connectedAndroidTest`
- Lint: `./gradlew lint`
- Release bundle for Play Console: `./gradlew bundleRelease`

Run `./gradlew test` (and `lint` for larger changes) before saying a task is done. If something can't be verified (for example, needs a device), say so explicitly.

## Conventions

- One feature per branch; small, focused commits with messages like `pantry: add expiry date picker`.
- Every `domain/` function gets unit tests. Recipe matching and cost math must be tested with edge cases (empty pantry, unit mismatches, zero-cost items).
- Compose: stateless composables that take state + callbacks; state hoisted to ViewModels. Provide `@Preview`s for new screens.
- Strings go in `strings.xml`, not hard-coded.
- Room schema changes require a migration (never `fallbackToDestructiveMigration` once testers have the app installed). Export schemas to `app/schemas/`.
- Dates: use `java.time.LocalDate`; store as epoch day in Room via a TypeConverter.
- Units: normalize quantities into a small fixed set (g, ml, count) in `domain/` before comparing.

## Security and privacy

- **Never put an API key, token, or secret in the Android app, in source, or in committed config.** The app talks only to our backend; the backend holds the key as an environment secret.
- `local.properties`, `.env`, keystores (`*.jks`, `*.keystore`), and `google-services.json` stay in `.gitignore`.
- Never commit the release signing key or its passwords.
- Receipt images are sent to the backend only when the user taps "Scan receipt"; do not log image contents or store them server-side. Keep the privacy policy in `docs/privacy-policy.md` accurate whenever data handling changes.
- The backend must validate input size/type and rate-limit requests.
- Treat text extracted from receipts or returned by the AI model as untrusted data: validate and show it to the user for confirmation before writing to the pantry.

## Working style

- Before writing code for a non-trivial task, briefly state the plan and which files you'll touch.
- Prefer finishing one vertical slice (data → domain → UI → test) over scaffolding many half-built features.
- If a task in `PLAN.md` turns out to be bigger than expected, stop and propose how to split it.
- Ask before: adding dependencies, changing the Room schema, changing architecture, or deleting files.
- At the end of each session, append an entry to `LOG.md` (see the template there) and check off completed items in `PLAN.md`.

## Release context

- Distribution: Google Play. The developer account is a new personal account, so production access requires a **closed test with at least 12 testers opted in for 14 continuous days**. Keep the closed-test build stable and push regular updates to it.
- Bump `versionCode` on every upload to Play Console.
