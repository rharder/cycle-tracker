# Cycle Tracker

A mobile-friendly menstrual cycle tracker that lives at
**https://rharder.github.io/cycle-tracker/**

---

## Setup (one-time)

### 1. Push files to GitHub

```bash
git clone https://github.com/rharder/cycle-tracker.git
cd cycle-tracker
cp /path/to/downloaded/cycle-tracker.html index.html
cp /path/to/downloaded/apple-touch-icon.png .
cp /path/to/downloaded/favicon.ico .
git add .
git commit -m "Initial commit"
git push
```

### 2. Enable GitHub Pages

1. Go to https://github.com/rharder/cycle-tracker/settings/pages
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**

Your app will be live at **https://rharder.github.io/cycle-tracker/** within a minute or two.

---

## Google Drive Sync Setup

This lets both of you sync data automatically using a shared Google account.

### Step 1 — Create a Google Cloud project

1. Go to https://console.cloud.google.com/
2. Click **Select a project** → **New Project**
3. Name it `cycle-tracker`, click **Create**

### Step 2 — Enable the Google Drive API

1. In your new project, go to **APIs & Services** → **Library**
2. Search for **Google Drive API** and click **Enable**

### Step 3 — Create an OAuth Client ID

1. Go to **APIs & Services** → **Credentials**
2. Click **+ Create Credentials** → **OAuth client ID**
3. If prompted, configure the **OAuth consent screen** first:
   - User type: **External**
   - App name: `Cycle Tracker`
   - Support email: your email
   - Scroll to the bottom, click **Save and Continue** through all steps
   - On the final screen, click **Back to Dashboard**
4. Back in Credentials → **+ Create Credentials** → **OAuth client ID**
   - Application type: **Web application**
   - Name: `Cycle Tracker Web`
   - Under **Authorized JavaScript origins**, add:
     ```
     https://rharder.github.io
     ```
   - Click **Create**
5. Copy the **Client ID** — it looks like `1234567890-abc123.apps.googleusercontent.com`

### Step 4 — Add your Client ID to the app

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

### Step 5 — Add your app to the OAuth consent screen test users

While your app is in "Testing" mode, only explicitly added users can sign in:

1. Go to **APIs & Services** → **OAuth consent screen**
2. Scroll to **Test users** → **+ Add Users**
3. Add the Google account email(s) you'll use to sign in

---

## Using the app

### Add to iPhone home screen

1. Open **https://rharder.github.io/cycle-tracker/** in Safari
2. Tap the **Share** button (box with arrow)
3. Tap **Add to Home Screen**
4. Tap **Add**

### Sync between devices

1. Open the app on either device
2. Go to **Settings** tab
3. Tap **Sign in with Google** and sign in with your shared Google account
4. Data syncs automatically — changes push to Drive within 2 seconds of any edit

The sync file is named `cycle-tracker-data.json` in the root of your Google Drive.
Both devices merge data on connect, so no history is lost when switching devices.

---

## File structure

```
index.html          — the entire app (self-contained)
apple-touch-icon.png — 1024×1024 home screen icon
favicon.ico         — browser favicon
README.md           — this file
```
