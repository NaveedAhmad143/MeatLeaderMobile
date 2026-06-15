# Build Instructions — APK Generation

This document explains exactly how to produce the `.apk` file required by the assignment.

---

## Prerequisites

1. **Node.js 18 or 20** — https://nodejs.org/
2. **Git** — https://git-scm.com/
3. (Path B only) **Android Studio** with Android SDK + JDK 17

Verify:
```bash
node --version    # v18+ or v20+
npm --version
```

---

## Step 0 — Install project dependencies

In the `MeatLedgerMobile/` folder:

```bash
npm install
```

This downloads Expo, React Navigation, expo-sqlite, expo-crypto, etc.
(First install takes 2–4 minutes.)

---

## Path A — Cloud APK build with EAS (recommended)

Easiest. No Android SDK or JDK needed on your machine. Build runs on Expo's servers.

```bash
# 1. install the EAS CLI globally
npm install -g eas-cli

# 2. log in (creates a free Expo account if you don't have one)
eas login

# 3. configure the project (one-time, accept defaults)
eas build:configure

# 4. run the build for an installable APK
eas build -p android --profile preview
```

The CLI prints a build URL. After ~5 minutes, the build page shows an **Install** button
to download the `.apk`. Transfer it to an Android phone and install.

---

## Path B — Local APK build with Gradle

Requires **Android Studio + Android SDK + JDK 17** on your machine. Takes 10–15 min.

```bash
# 1. generate the android/ folder
npx expo prebuild --platform android

# 2. build a release APK
cd android
./gradlew assembleRelease       # Linux / macOS
# OR
gradlew.bat assembleRelease     # Windows
```

The APK lands at:
```
android/app/build/outputs/apk/release/app-release.apk
```

Copy that to your phone and install (you may need to allow "Install from unknown sources").

---

## Running on an emulator or real device for testing

You don't need an APK to test — Expo can run it directly:

```bash
npx expo start
```

Then:
- Press `a` to open on an Android emulator (Android Studio's AVD must be running), or
- Scan the QR code with the **Expo Go** app on a real Android phone.

---

## Troubleshooting

**`expo-sqlite` errors during install** — make sure you have Expo SDK 51 installed
exactly (`expo@~51.0.28`). Run `npx expo doctor` to check compatibility.

**Gradle build fails with "SDK location not found"** — open `android/local.properties`
and add `sdk.dir=C:/Users/<you>/AppData/Local/Android/Sdk` (Windows) or your SDK path.

**EAS build queues for a long time** — the free tier has a queue. The build itself
takes ~5 min once it starts.

---

## Submission

For the assignment, include:

1. **Source code** — push this `MeatLedgerMobile/` folder to GitHub, share the link
2. **APK file** — from Path A or B above
3. **Project report PDF** — see `MeatLedger-Report.pdf`
