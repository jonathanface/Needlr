# MiniDocter

Mobile companion app for [Threadr](../threadr) — the fiction-writing tool with automatic entity highlighting (characters, locations, events, etc.). This app brings the same editor and story management to iOS and Android.

- **Internal name:** `minithreadr`
- **Store package:** `io.docter.mobile`

## Stack

- **Framework:** Expo SDK 54 on React Native 0.81.5, React 19
- **Language:** TypeScript
- **Editor:** The Threadr Lexical editor runs inside a WebView (`react-native-webview`). The `lexical-editor/` workspace builds to HTML/JS that gets bundled into the app via `scripts/bundle-lexical-html.js` and consumed from `src/lexical-html/`.
- **Navigation:** React Navigation v7 (native stack)
- **Auth / secrets:** `expo-auth-session` + `expo-secure-store` (OAuth flow matching the Threadr backend)
- **Storage:** `@react-native-async-storage/async-storage`
- **Testing:** Jest + `@testing-library/react-native`

## Repo layout

```
App.tsx / index.ts    Entry points
src/
  components/         Reusable UI components
  config/             App configuration
  contexts/           React contexts (auth, theme, etc.)
  hooks/              Custom hooks
  lexical-html/       Bundled Lexical editor assets (output of lexical-editor/)
  screens/            Top-level screens routed by React Navigation
  theme/              Theming (supports dark/light mode)
  types/              Shared TypeScript types
  utils/              Helpers
lexical-editor/       Workspace package — the embedded Lexical editor
android/              Native Android project (gradle, keystore)
ios/                  Native iOS project (Xcode workspace, Podfile)
scripts/              Build helpers (incl. bundle-lexical-html.js)
__mocks__/            Jest mocks
```

## Development

```
npm install                 # Install dependencies (workspaces)
npm run build:lexical       # Build the Lexical workspace and bundle its HTML
npm start                   # Expo dev server
npm run start-clear         # Rebuild Lexical and start with cache cleared
npm run android             # Build + run on Android
npm run ios                 # Build + run on iOS
npm run web                 # Expo web dev server
npm run typecheck           # tsc --noEmit
npm test                    # Jest
npm run test:watch
npm run test:coverage
npm run precheck            # Pre-push checks (scripts/pre-push-check.sh)
```

When editor behavior changes, re-run `npm run build:lexical` so `src/lexical-html/` picks up the new bundle.

## Release

Both platforms are configured for store submission (see `STORE_SUBMISSION.md`):

- Android: AAB build, signing keystore `@jface78__MiniDocter.jks` at repo root.
- iOS: Release configuration via EAS (`eas.json`).

## Related

- [Threadr](../threadr) — Go backend + React web frontend. This mobile app authenticates against and reads/writes the same stories.
