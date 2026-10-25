# Publishing guide: Google Play and Apple App Store

## What you need
| Item | Google Play | Apple App Store |
|---|---|---|
| Developer account | One time US$25 | US$99 per year |
| Computer | Any (or GitHub Actions) | A Mac with Xcode is needed for final signing and upload |
| Identity check | Government ID, and a phone and address | Government ID, and a D-U-N-S number only if you register as a company |
| Review time | A few days to a week | Usually 1 to 3 days |

New personal Play accounts must run a **closed test with at least 12 testers for 14 days** before they can apply for production. Start recruiting friends early.

## Step 0: host the privacy policy
Both stores need a public URL. Put `docs/privacy.html` on GitHub Pages: repository Settings > Pages > Branch main, folder /docs. The URL becomes `https://YOURNAME.github.io/REPO/privacy.html`.

## Android
1. Create the Play Console account and a new app (Free, Game, English).
2. Make a signing key once and keep it safe forever:
   `keytool -genkeypair -v -keystore release.jks -alias chemist -keyalg RSA -keysize 2048 -validity 10000`
3. Add three GitHub repository secrets: `ANDROID_KEYSTORE_B64` (output of `base64 -w0 release.jks`), `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_ALIAS` (use `chemist`).
4. Run the "Android build" workflow. Download `app-release.aab` from the run.
5. In Play Console: Testing > Closed testing > create a release > upload the .aab. Add testers by email. Fill in Store listing (from STORE_LISTING.md), Content rating, Data safety, Target audience (18+ is simplest, or 13+; choosing under 13 triggers Families policy) and the privacy policy URL.
6. After 14 days with 12 testers, apply for production access, then publish.
Turn on Play App Signing when asked.

## iOS
1. Join the Apple Developer Program. In App Store Connect create the app with bundle ID `com.subhanjan.chemisttycoon`.
2. On a Mac: `npm install`, `npx cap add ios`, `npx capacitor-assets generate --ios`, `npx cap sync ios`, `npx cap open ios`.
3. In Xcode choose the App target > Signing and Capabilities > your Team, tick Automatically manage signing. Set the version to 1.0 and build to 1.
4. Choose Any iOS Device (arm64), then Product > Archive > Distribute App > App Store Connect > Upload.
5. Test with TestFlight on your own iPhone first.
6. In App Store Connect add screenshots, description, keywords, privacy URL, App Privacy (Data Not Collected), age rating, and submit for review.
Without a Mac you can rent a cloud Mac or use a service such as Codemagic or Xcode Cloud. The included `ios.yml` only checks that the project compiles.

## Likely review issues, and how this app avoids them
- **Apple 4.2 minimum functionality (web wrapper rejection):** the game is a full offline interactive app with saves, not a website in a shell. In review notes say: "A fully offline strategy simulation game."
- **Medical content:** the app is a business simulation and gives no medical advice. The description says so. Answer the age rating questions as a game.
- **Government and company names:** the app builds already replace the Jan Aushadhi brand with a generic "Government generics shop", and the game names no real companies.
- **Privacy:** fonts are bundled, so the app makes no network calls at all.
- **Support URL and contact email** are required: use the repository page and your email.

## After launch
Update by replacing `www/classic.html` or `www/real.html`, bumping the version (Android build number is automatic), then run the workflow or archive in Xcode again.
