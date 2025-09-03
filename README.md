# UPSC Prelims Quick Mock (React + Vite + Capacitor)

A lightweight UPSC Prelims mock test web app built with React (Vite + TypeScript) and styled with Tailwind. It supports question generation via Google Gemini and ships as a web app or an Android APK using Capacitor.

## Features

- Geography, History, Political Science, International Relations, Economy, Science & Tech, Current Affairs
- Rich syllabus model: each chapter contains concepts and topics for targeted question generation
- React Router v7 with `createBrowserRouter` + `RouterProvider`
- Tailwind CSS UI
- Persist recent results in LocalStorage
- Android build via Capacitor

## Tech Stack

- React + Vite + TypeScript
- React Router v7
- Tailwind CSS
- Axios
- Capacitor (Android)

## Project Structure (key files)

- `src/App.tsx` — Router setup using `createBrowserRouter`
- `src/pages/Home.tsx` — Create test: choose chapter, concepts, difficulty
- `src/pages/Test.tsx` — Take the test with timer and scoring
- `src/pages/Result.tsx` — Summary screen and recent tests
- `src/utils/chapters.ts` — UPSC syllabus data: concepts + topics
- `src/utils/geminiApi.ts` — Gemini prompt + parsing

## Prerequisites

- Node 18+ (Node 20/22 recommended)
- npm 9+
- (Optional, for Android) Android Studio + Java 17
- Google Gemini API key

## Environment Variables

Create a `.env` (or `.env.local`) in the project root:

```
VITE_GEMINI_KEY=your_gemini_api_key
```

## Install & Run (Web)

```bash
npm install
npm run dev
```

Build for production:

```bash
npm run build
npm run preview
```

## Android (via Capacitor)

One-time (already initialized here, for reference):

```bash
npm install @capacitor/core @capacitor/cli @capacitor/android
npx cap add android
```

Regular workflow:

```bash
# 1) Build web
npm run build

# 2) Sync web assets to Android
npx cap sync android

# 3) Open in Android Studio
npx cap open android
```

From Android Studio, select a device/emulator and click Run to install. Debug logs are available in Logcat.

CLI build (PowerShell on Windows):

```bash
cd android
.\gradlew.bat assembleDebug
```

APK will be at:
- Debug: `android/app/build/outputs/apk/debug/app-debug.apk`
- Release: `android/app/build/outputs/apk/release/app-release.apk`

If you see a Java 21 source error, set Java 17 in `android/app/capacitor.build.gradle` (already configured) and use JDK 17 in Android Studio.

## Data Model (chapters.ts)

`chapters` is organized by subject → chapter → array of concept objects. Each concept has a list of topics for focused question prompts.

Example:

```ts
{
  geography: {
    "Physical Geography": [
      {
        concept: "Earth's structure and composition",
        topics: ["Crust, mantle, core", "Plate boundaries", "Earthquakes", "Volcanoes", "Minerals and rocks"]
      }
    ]
  }
}
```

## Question Generation (Gemini)

- Implemented in `src/utils/geminiApi.ts`
- Builds a prompt that includes the selected chapter, concepts, and topics
- Expects JSON array: `{ question: string, options: string[4], answer: number }`

Tips:
- Ensure `VITE_GEMINI_KEY` is set
- The API returns free-form text; the code extracts and parses the first JSON array

## Routing

- Uses React Router v7 with `createBrowserRouter` + `RouterProvider` (see `src/App.tsx`)
- For static hosting that doesn’t support SPA rewrites, consider `createHashRouter`

## Common Issues & Fixes

- Pages only load after refresh
  - Fixed by using React Router v7 `RouterProvider`. For static hosts, use hash router or add a 404.html fallback to route all paths to `index.html`.

- TypeScript: Property 'map' does not exist on type 'never'
  - Cast dynamic chapter lists to arrays when mapping: `(subject[selected] as any[]).map(...)`

- Missing `chapters` in `geminiApi.ts`
  - Import with `import { chapters } from "./chapters";`

- Android Gradle Java version error (source 21)
  - Use Java 17 and ensure `android/app/capacitor.build.gradle` sets `JavaVersion.VERSION_17`

## Scripts

- `npm run dev` — Start dev server
- `npm run build` — Production build
- `npm run preview` — Preview production build
- `npm run deploy` — Deploy to GitHub Pages (adjust as needed)

## License

MIT


