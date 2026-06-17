# Deployment & Developer Guide

This document covers how to host your own instance of Cycle Tracker and set up Google Drive sync.

---

## Hosting your own copy

### 1. Fork or clone the repo

```bash
git clone https://github.com/rharder/cycle-tracker.git
cd cycle-tracker
```

### 2. Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**

Your app will be live at `https://<your-username>.github.io/cycle-tracker/` within a minute or two.

---

## Google Drive Sync Setup

The app ships with sync disabled. To enable it you need a Google OAuth Client ID.
Each deployment needs its own — you cannot reuse someone else's.

### Step 1 — Create a Google Cloud project

1. Go to https://console.cloud.google.com/
2. Click **Select a project** → **New Project**
3. Name it `cycle-tracker`, click **Create**

### Step 2 — Enable the Google Drive API

1. Go to **APIs & Services** → **Library**
2. Search for **Google Drive API** and click **Enable**

### Step 3 — Configure the OAuth consent screen

1. Go to **APIs & Services** → **OAuth consent screen**
2. User type: **External**, click **Create**
3. Fill in:
   - App name: `Cycle Tracker`
   - Support email: your email
   - Developer contact: your email
4. Click through **Save and Continue** on all remaining steps

### Step 4 — Create an OAuth Client ID

1. Go to **APIs & Services** → **Credentials**
2. Click **+ Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: `Cycle Tracker Web`
5. Under **Authorized JavaScript origins** add:
   ```
   https://<your-username>.github.io
   ```
6. Click **Create** and copy the **Client ID**

### Step 5 — Add the Client ID to the app

Open `index.html` and find this line near the top of the `<script>` section:

```javascript
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID';
```

Replace `YOUR_GOOGLE_CLIENT_ID` with your actual Client ID, then push:

```bash
git add index.html
git commit -m "Add Google OAuth client ID"
git push
```

### Step 6 — Add test users (while in Testing mode)

While your OAuth app is in Testing mode, only explicitly listed users can sign in:

1. Go to **APIs & Services** → **OAuth consent screen**
2. Scroll to **Test users** → **+ Add Users**
3. Add the Google account email(s) you'll use

To allow anyone to sign in, publish the app by clicking **Publish App** on the consent screen. For a personal tracker used by just one or two people, staying in Testing mode is fine.

---

## Regenerating the icon files

The icons are generated with Pillow. To regenerate them:

```bash
pip install Pillow
python3 gen_icons.py   # not included — see icon generation notes below
```

The icon is a four-segment pie chart representing the four cycle phases, clipped to Apple's squircle shape. Segments are proportional to average phase lengths (5/9/3/11 days).

---

## File structure

```
index.html            — entire app, self-contained
apple-touch-icon.png  — 1024×1024 iOS home screen icon
favicon.ico           — multi-size browser favicon (16, 32, 48, 256)
README.md             — end-user documentation
DEPLOY.md             — this file
```

---

## Tech notes

- Pure HTML/CSS/JS — no build step, no dependencies, no framework
- Data stored in `localStorage` under key `cycle_tracker_v2`
- Google Drive sync uses the [Google Identity Services](https://developers.google.com/identity/oauth2/web) token flow
- Drive data stored as `cycle-tracker-data.json` in the user's Drive root
- Sync is debounced 2 seconds after any data change
- On connect, data is merged (union by period start date) — no data is ever overwritten or lost
