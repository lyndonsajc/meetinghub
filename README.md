# Meeting AI Hub

A deploy-ready single-file app for recording meetings/consultations, generating structured AI-style summaries from notes, saving records, tracking action items, exporting email drafts, and syncing records across devices using Firebase Firestore.

## Files

- `index.html` — main app
- `vercel.json` — Vercel static deployment config
- `firebase.json` — Firebase Hosting + Firestore rules config
- `firestore.rules` — Firestore security rules
- `.firebaserc` — replace `YOUR_FIREBASE_PROJECT_ID` with your Firebase project ID

## What changed

Firestore sync has been added. The app now:

- saves records locally first
- connects to Firestore when Firebase config is added
- syncs records across browsers/devices
- updates action-item completion status in Firestore
- deletes records from Firestore when deleted in the app
- falls back to localStorage if Firebase is not configured

## Firebase setup

1. Go to Firebase Console.
2. Create or open your project.
3. Build → Firestore Database → Create database.
4. Build → Authentication → Sign-in method → enable **Anonymous**.
5. Project settings → General → Your apps → Web app.
6. Copy the Firebase config object.
7. Open the deployed app → Export page → Firestore Sync.
8. Paste the config JSON and click **Connect Firestore**.

Example config format:

```json
{
  "apiKey": "...",
  "authDomain": "...firebaseapp.com",
  "projectId": "...",
  "storageBucket": "...appspot.com",
  "messagingSenderId": "...",
  "appId": "..."
}
```

## Firestore rules

This package includes `firestore.rules`, which allows read/write only after anonymous Firebase sign-in:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /meetingRecords/{recordId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

For a private staff-only app later, replace anonymous auth with Google sign-in and restrict access by email domain.

## Push to GitHub

```bash
git init
git add .
git commit -m "Add Firestore sync to Meeting AI Hub"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/meeting-ai-hub.git
git push -u origin main
```

## Deploy to Vercel

1. Go to Vercel.
2. Add New Project.
3. Import the GitHub repository.
4. Framework preset: Other.
5. Build command: leave blank.
6. Output directory: leave blank.
7. Deploy.

## Deploy Firebase Hosting + Rules

Install Firebase CLI if needed:

```bash
npm install -g firebase-tools
firebase login
```

Edit `.firebaserc` and replace `YOUR_FIREBASE_PROJECT_ID`.

Then deploy:

```bash
firebase deploy
```

## Important note

Vercel can host the app, while Firebase provides Firestore sync. Firebase Hosting is optional if you prefer Vercel.
