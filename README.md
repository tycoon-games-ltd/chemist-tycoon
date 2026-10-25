# Chemist Tycoon (Android and iOS app)

A Capacitor wrapper around the two game editions. Fully offline, no network calls, separate saves per edition.

- Publishing to Google Play and the App Store: read `PUBLISHING.md`.
- Store text, keywords and answers to the privacy questionnaires: `STORE_LISTING.md`.
- Privacy policy page to host: `docs/privacy.html`.

## Quick build (Android debug APK on GitHub)
1. Upload this folder to a new GitHub repository (keep `.github`).
2. Actions > "Android build" > Run workflow.
3. Download `app-debug.apk` from the run and install it on your phone.

## Build locally
Needs Node 18+, JDK 17, Android Studio (Android) or Xcode (iOS).
```
npm install
npx cap add android      # and/or: npx cap add ios
npx capacitor-assets generate
npx cap sync
npx cap open android     # or: npx cap open ios
```
Icons and splash screens come from `assets/`. Fonts are bundled in `www/fonts`.

## Updating a game
Replace `www/classic.html` or `www/real.html`, run `npx cap sync`, rebuild.
