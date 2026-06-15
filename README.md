# MeatLedger Mobile

A React Native point-of-sale and inventory app for butcher shops and specialty meat/seafood markets. Built with Expo, expo-sqlite, and an on-device AI assistant.

**Course Assignment #2 — Mobile Application Development**

## Features

- Authentication (Sign Up / Sign In) — passwords are SHA-256 hashed before storage
- Dashboard with daily KPIs and AI insights
- Sales Counter (weight-based per-pound billing) with cash / card / credit payment
- Inventory management with stock intake and reorder alerts
- Customer management with credit accounts (receive payments)
- Reports & analytics over selectable time ranges (Today / 7d / 14d / 30d)
- **AI Assistant** (unique feature) — rule-based, on-device, answers questions in natural English

## Tech Stack

- React Native (via Expo SDK 51)
- expo-sqlite — local SQLite database (this is the **database connectivity** required by the assignment)
- expo-crypto — SHA-256 password hashing
- @react-navigation — stack + bottom tab navigation
- No external API or cloud service — app runs fully offline

## Database

The app uses an on-device SQLite database (`meatledger.db`). Tables:

- `users` — authentication (id, name, email, password_hash)
- `products` — catalogue (name, category, cost, price, stock, reorder)
- `customers` — customer directory (name, phone, type, credit, spent, visits)
- `sales` — sales header (datetime, total, profit, payment, customer_id, user_id)
- `sale_items` — sale line items (sale_id, product_id, weight, price, amount)

On first run, the database is seeded with 15 demo products across 4 categories
(Beef, Lamb, Chicken, Seafood), 6 demo customers, and 14 days of demo sales —
so reports, charts, and the AI assistant have data to work with immediately.

## Unique Feature: AI Assistant

`src/ai.js` implements an on-device, rule-based assistant that reads the SQLite
tables and answers natural-language questions like:

- "Today's summary" / "Today's profit"
- "Tomorrow's demand forecast"
- "Pricing check" / "Margin check"
- "Low stock" / "Reorder"
- "Top customers"
- "Best seller this week"
- "Credit accounts"
- "Daily briefing"

It's **fully offline** — no OpenAI/Gemini key needed, nothing leaves the device.
The intelligence comes from keyword routing + SQL aggregations + business
heuristics. See `src/ai.js` for the full implementation.

## Quick Start (run locally)

Requires Node.js 18+ and Android Studio (for the Android emulator) or the
Expo Go app on a physical device.

```bash
# 1. install dependencies
npm install

# 2. start the Expo dev server
npx expo start

# 3. press 'a' to open on Android emulator, or scan the QR with Expo Go
```

## Building the APK

There are two ways to produce an APK.

### Option A — Cloud build with EAS (recommended, ~5 min, no Android SDK needed)

```bash
npm install -g eas-cli
eas login                    # one-time, free Expo account
eas build -p android --profile preview
```

The CLI prints a download link when the build finishes. The `preview` profile
produces an installable APK (not a Play-Store AAB).

### Option B — Local build with Gradle (~10–15 min, requires Android SDK + JDK 17)

```bash
# generate the android/ folder
npx expo prebuild --platform android

# build a release APK
cd android
./gradlew assembleRelease

# APK lands here:
# android/app/build/outputs/apk/release/app-release.apk
```

On Windows, replace `./gradlew` with `gradlew.bat`.

## Project Structure

```
MeatLedgerMobile/
├── App.js                       # Root, navigation, auth gate
├── index.js                     # Expo entry
├── package.json                 # Dependencies
├── app.json                     # Expo config (name, icon, package)
├── babel.config.js
├── src/
│   ├── theme.js                 # Colors and shared styles
│   ├── db.js                    # SQLite init, schema, seed, queries
│   ├── auth.js                  # AuthContext + signUp / signIn
│   ├── ai.js                    # AI assistant (unique feature)
│   ├── ui.js                    # Shared components (Btn, Card, KPI, Badge)
│   └── screens/
│       ├── SignInScreen.js
│       ├── SignUpScreen.js
│       ├── DashboardScreen.js
│       ├── POSScreen.js
│       ├── InventoryScreen.js
│       ├── CustomersScreen.js
│       ├── ReportsScreen.js
│       ├── AIScreen.js
│       └── SettingsScreen.js
└── assets/
    └── icon.png
```

## License

Prototype / educational use.
