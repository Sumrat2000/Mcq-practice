# MCQ Practice — Android App (Capacitor project)

This folder is a ready-to-build Capacitor project. It wraps the MCQ Practice
web app (in `www/index.html`) into a real Android app you can compile into an APK.

## What you need installed on your computer (one-time setup)

1. **Node.js** (v18 or later) — https://nodejs.org
2. **Android Studio** — https://developer.android.com/studio
   (This also installs the Android SDK and Java automatically.)

## Steps to build the APK

Open a terminal in this project folder and run:

```bash
# 1. Install dependencies
npm install

# 2. Add the Android platform
npx cap add android

# 3. Copy the web app into the Android project
npx cap sync

# 4. Open the project in Android Studio
npx cap open android
```

Android Studio will open. Then:

1. Wait for Gradle to finish syncing (bottom status bar).
2. Go to **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
3. When it finishes, click the **"locate"** link in the notification popup —
   that's your `app-debug.apk`.
4. Copy that APK to your phone (email it to yourself, use a USB cable, or
   Google Drive) and open it there.
5. Your phone will ask you to allow "install from unknown sources" the first
   time — allow it, then tap the APK to install.

## Building the APK from your phone using GitHub Actions (no Android Studio needed)

This project includes `.github/workflows/build-apk.yml`, which builds the
APK for you automatically in the cloud whenever you push to GitHub. This is
the easiest path if you're working entirely from Termux on your phone.

### One-time setup

1. Install Termux and these packages:
   ```bash
   pkg install git
   ```
2. Create a free account at https://github.com if you don't have one.
3. On github.com, create a new **empty** repository (no README, no .gitignore) —
   e.g. named `mcq-practice`.
4. On GitHub, go to **Settings → Developer settings → Personal access tokens →
   Tokens (classic)** and generate a token with the `repo` scope. Copy it —
   you'll use it as your password when pushing. (GitHub no longer accepts
   your account password directly over git.)

### Push this project from Termux

Unzip this project on your phone, `cd` into the `mcq-app` folder in Termux, then:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/mcq-practice.git
git push -u origin main
```

When it asks for a username/password, use your GitHub username and paste the
**personal access token** as the password.

### Get your APK

1. Go to your repo on github.com (in your phone's browser) → the **Actions** tab.
2. You'll see a "Build Android APK" run in progress (it takes a few minutes).
3. Once it finishes (green checkmark), open the run → scroll down to
   **Artifacts** → tap **mcq-practice-apk** to download it as a zip.
4. Unzip it (Termux: `unzip mcq-practice-apk.zip`) to get `app-debug.apk`.
5. Move/share that APK to your phone's file manager and tap it to install
   (allow "install from unknown sources" if prompted).

### Updating the app later

Whenever you edit `www/index.html`, just:
```bash
git add .
git commit -m "Update app"
git push
```
GitHub Actions will automatically rebuild the APK — no local build tools needed.

## Notes

- This produces a **debug APK**, good for installing on your own phone.
  If you ever want to publish it on the Google Play Store, you'll need to
  create a **signed release build** (Android Studio can walk you through
  this under Build → Generate Signed Bundle/APK) and a $25 one-time
  Google Play developer account.
- If `npx cap add android` fails asking for a Java version, install the
  JDK version Android Studio recommends (it usually installs one for you
  under Android Studio's own settings — set `JAVA_HOME` to that).
- To update the app later, just edit `www/index.html`, then re-run
  `npx cap sync` and rebuild in Android Studio.
