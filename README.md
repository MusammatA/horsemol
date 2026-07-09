# horsemol

horsemol is an Altai-inspired productivity web app that combines music, focus tools, and personal workspace features in a single-page experience. The project blends ambient background imagery, curated playlists, timers, journaling, sticky notes, and lightweight account syncing into a focused environment for study, work, and creative sessions.

Live site: [horsemol.vercel.app](https://horsemol.vercel.app)

## Overview

The app is designed as a static frontend with no build step. Most workspace data is stored locally in the browser so the experience remains fast and simple. Optional Google sign-in, powered by Firebase Authentication and Firestore, allows users to save their profile and focus summary across devices.

The experience is also shaped by Altai cultural inspiration, especially the storytelling power of music, epic figures, and steppe imagery.

## Core Features

- Rotating full-screen background photography with smooth transitions.
- Music player powered by YouTube playlists, including play/pause, skip, seek, auto-advance, and upcoming-track display.
- Focus timer system with `25 / 5`, `50 / 15`, `90 / 20`, and custom focus/break sessions.
- Multiple alarm options, including a standard alarm, Altai-themed alarm, and gentle alarm preview.
- Persistent sticky notes with drag, resize, color selection, delete, and undo support.
- Persistent to-do list with add, complete, delete, and scroll behavior.
- Journal interface with page-turn navigation and notebook-style writing layout.
- Google account sign-in for profile management and focus summary syncing.
- Altai information panel and warrior profile selection.
- Responsive layout tuned for desktop and mobile screens.

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Firebase Authentication
- Cloud Firestore
- YouTube IFrame Player API
- Vercel for deployment

## Project Structure

```text
.
├── index.html                 # Main single-page application
├── README.md
├── firebase.json              # Firebase project config used for Firestore rules
├── firestore.rules            # Firestore access control rules
├── assets/
│   └── alarms/
│       ├── gentle-pixel-3.mp3
│       └── throat-singing.mp3
├── journal-cover.png
├── gold-ornament.svg
├── griff-runner.gif
├── *.jpg                      # Backgrounds and warrior images
```

## Local Development

There is no package install or build pipeline required.

1. Clone the repository.
2. Start a local static server from the project root.
3. Open the app in a browser using `localhost`.

Example:

```bash
cd horsemol
python3 -m http.server 8123
```

Then open:

```text
http://127.0.0.1:8123
```

Note: Google sign-in does not work from `file://` URLs. Use `localhost` locally, or an authorized HTTPS domain in production.

## Firebase Setup

Firebase is only required for account syncing and cross-device profile storage. The rest of the workspace can still function locally in the browser without sign-in.

### 1. Create a Firebase project

- Create a Firebase project in the Firebase console.
- Add a Web App to the project.
- Copy the Firebase web configuration.

### 2. Enable Google Authentication

- Open `Authentication` in Firebase.
- Enable `Google` as a sign-in provider.
- Add your deployment domain to `Authorized domains`.
- Keep `localhost` authorized for local development.

### 3. Create Firestore

- Create a Cloud Firestore database.
- Use production mode if you want secure default behavior.

### 4. Apply Firestore Rules

The project includes Firestore rules in [`firestore.rules`](./firestore.rules). These rules only allow a signed-in user to read and update their own profile document.

### 5. Provide the Firebase Web Config

The app reads config from `window.HORSEMOL_FIREBASE_CONFIG` if it exists, and otherwise falls back to the inline config currently in [`index.html`](./index.html).

Example:

```html
<script>
  window.HORSEMOL_FIREBASE_CONFIG = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT.firebasestorage.app",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
  };
</script>
```

## Data Storage Model

The app currently uses two storage layers:

- `localStorage` for journal pages, sticky notes, to-do items, and local site settings.
- Firestore for signed-in account profile data and focus summary data.

This keeps the app usable without an account while still allowing optional persistence across devices.

## Security Notes

- Firebase web app configuration values are public by design and are safe to expose in the client.
- The real security boundary is enforced through Firebase Authentication and [`firestore.rules`](./firestore.rules).
- Firestore access is restricted to the authenticated owner of a profile document.
- All other Firestore reads and writes are denied by default.
- The app includes a restrictive Content Security Policy and Permissions Policy in the page head.

## Deployment

The project can be deployed as a static site on Vercel.

Recommended setup:

- Framework preset: `Other`
- Root directory: project root
- Build command: none
- Output directory: none

After deployment:

1. Confirm the site loads over HTTPS.
2. Add the deployed domain to Firebase Authentication authorized domains.
3. Verify Google sign-in and Firestore profile writes on the live site.

## Browser Behavior Notes

- Due to browser autoplay restrictions, the user may need to interact with the page once before music playback begins.
- YouTube playback depends on the YouTube IFrame API being available.
- Sign-in flows depend on popups or redirect support in the browser.

## About the Developer

Hi, I'm Musammat Aktar, a Mathematics and Computer Science student at Columbia University from Detroit, Michigan. I enjoy building projects that combine technology with subjects that do not usually meet.

One of those interests is the culture of the Altai people. Their music, shaped by both Turkic and Mongolic traditions, often tells stories of legendary warriors, folklore, and life on the steppe. horsemol is an attempt to bring those influences into a focused digital workspace.
