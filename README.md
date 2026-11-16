# Larder

> Working name — subject to change.

**Know what's in your kitchen, use it before it goes bad, and eat well on a budget with the equipment you actually have.**

Larder is an Android app for students and anyone cooking on a tight budget. It tracks your pantry and expiry dates, reminds you to use food before it spoils, suggests meals you can make from what you already have with the equipment you own, and plans a one-trip weekly shop.

> 🚧 **Status:** In active development (November–December 2026). Currently in Google Play closed testing. See [PLAN.md](PLAN.md) for progress.

---

## Features

| Feature | Status |
|---|---|
| **Pantry** — track items, quantities, location (fridge/freezer/pantry), and expiry dates | Planned · Week 1 |
| **Use it soon** — list and daily notification for items about to expire | Planned · Week 1 |
| **Meal suggestions** — recipes ranked by what's in your pantry, prioritizing expiring items, filtered by your equipment | Planned · Week 1 |
| **Weekly meal-prep plan** — pick meals that share ingredients so one shopping trip covers the week | Planned · Week 2 |
| **Smart shopping list** — only what's missing from your plan | Planned · Week 2 |
| **Cost tracking** — cost per serving and a weekly budget | Planned · Week 2 |
| **Receipt scan (AI)** — photograph a receipt and review the items it finds before adding them to your pantry | Planned · Week 3 |
| **Money saved** — value of food you used instead of wasted | Planned · Week 3 |

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

Larder is in **closed testing** on Google Play. To join as a tester, contact me with the Google account you use on your Android phone: `<your contact here>`.

Testers need to stay opted in for at least 14 days (a Google Play requirement for new developer accounts). Feedback is welcome through the in-app **Send feedback** link.

## Tech stack

- **Language:** Kotlin
- **UI:** Jetpack Compose, Material 3, Navigation Compose
- **Architecture:** MVVM (Compose screens → ViewModels with StateFlow → repositories → Room)
- **Persistence:** Room (local-first; no account required)
- **Background work:** WorkManager for daily expiry reminders
- **Serialization:** kotlinx.serialization
- **Networking:** Retrofit + OkHttp (AI backend only)
- **AI backend:** a small serverless function that holds the API key, so no secrets ship in the app
- **Min SDK:** 26 (Android 8.0)

## Project structure

```
app/src/main/java/<package>/
  ui/             Compose screens, one subpackage per feature
  data/           Room database, entities, DAOs, repositories
  domain/         Pure Kotlin logic (recipe matching, plan building, cost math)
  notifications/  WorkManager workers and notification channels
  ai/             Client for the AI backend
  AppContainer.kt Manual dependency injection
app/src/main/assets/
  recipes.json    Bundled budget recipes
  shelf_life.json Default shelf-life days by category
backend/          Serverless function for receipt scanning
docs/             Privacy policy, screenshots
```

Business logic lives in `domain/` with no Android dependencies, so it's covered by fast JVM unit tests.

## Building from source

**Requirements:** Android Studio (latest stable) and JDK 17+.

```bash
git clone <repo-url>
cd larder
./gradlew assembleDebug        # Windows: gradlew.bat assembleDebug
```

Or open the project in Android Studio and run the `app` configuration on an emulator or device.

**Tests and checks:**

```bash
./gradlew test                  # unit tests
./gradlew connectedAndroidTest  # instrumented tests (emulator/device required)
./gradlew lint
```

### Backend (optional, for receipt scanning)

The app works without the backend. Only the receipt-scan feature needs it.

1. See `backend/README.md` for setup on the chosen platform.
2. Set the AI provider API key as an **environment secret** on the platform. Never commit it.
3. Put the backend URL in `local.properties` (not committed):
   ```
   backend.url=https://your-backend.example.com
   ```

## Privacy

- Pantry data is stored **only on your device**.
- Receipt photos are sent to the backend **only when you tap "Scan receipt"**. They are processed to extract items and are not stored.
- Full policy: [docs/privacy-policy.md](docs/privacy-policy.md)

## How this was built

This app is my final project ("The Moonshot") for **TCSS 490: Generative AI and Software Development** at the University of Washington Tacoma, Autumn 2026. It was built with **Claude Code** as an AI coding agent, and I directed the work through a planned, logged workflow:

- [CLAUDE.md](CLAUDE.md): standing instructions for the agent (stack, conventions, security rules)
- [PLAN.md](PLAN.md): week-by-week plan with checkboxes
- [LOG.md](LOG.md): session-by-session log of decisions, agent notes, and tester feedback

## Roadmap

After the course project, possible next steps include a shared pantry for roommates, a potluck/group meal planner, barcode scanning, a larger recipe library, and a home-screen widget. See the Stretch goals section in [PLAN.md](PLAN.md).

## License

_TBD._ Choose one before making the repo public (for example, MIT or Apache 2.0).
