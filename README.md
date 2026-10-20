# COSMO LEAGUE 4.0-2026 auction: Firebase + Vercel setup

Viewers (team owners) need no account. Only you, the host, log in.

## 1. Firebase (free, ~10 minutes)
1. Go to https://console.firebase.google.com and click **Add project** (analytics not needed).
2. **Build > Realtime Database > Create database** (any region, start in **locked mode**).
3. **Build > Authentication > Get started > Email/Password > Enable**.
4. In Authentication, open the **Users** tab, click **Add user**, and enter your host email and a password. Copy the **User UID** that appears.
5. Back in Realtime Database, open the **Rules** tab, paste this (replace PASTE_HOST_UID with the UID), and click **Publish**:

```json
{
  "rules": {
    "auction": {
      ".read": true,
      ".write": "auth.uid === 'PASTE_HOST_UID'"
    }
  }
}
```
6. **Project settings (gear icon) > Your apps > Web (</>)**: register an app and copy the config values. The Realtime Database page shows the `databaseURL` at the top.
7. Open `index.html`, find `FIREBASE_CONFIG` near the top of the script, and paste in `apiKey`, `authDomain`, `databaseURL`, `projectId`, `appId`. (These values are not secrets; the rules above protect writing.)

## 2. Vercel
Option A, command line (needs Node.js): in this folder run `npx vercel --prod`, log in, accept the defaults (no build command, no output directory).
Option B, GitHub: push this folder to a GitHub repo, then on vercel.com choose **Add New > Project**, import the repo, and deploy with default settings.

## 3. Use it
Open your Vercel link, click **Host login**, sign in with the email and password from step 4. The first host login loads the 5 teams and 55 players into the database. Send the same link to team owners; they see live updates with no login.
