# Cycle Tracker

A free, private menstrual cycle tracker you can use right now — no app store, no account, no data collection.

**→ Open the app: [rharder.github.io/cycle-tracker](https://rharder.github.io/cycle-tracker/)**

---

## What it does

Cycle Tracker helps a person (or their partner) log menstrual periods and visualize the four phases of the cycle across several months:

| Color | Phase | Typical length |
|---|---|---|
| 🟠 Coral | Menstrual | ~5 days |
| 🟢 Teal | Follicular | ~9 days |
| 🟣 Purple | Ovulation | ~3 days |
| 🟡 Amber | Luteal | ~11 days |

As you log more cycles, the app automatically calculates your actual average cycle length and adjusts predictions to match.

---

## How to use it

### On iPhone (recommended)
1. Open **https://rharder.github.io/cycle-tracker/** in **Safari**
2. Tap the **Share** button (the box with an arrow at the bottom)
3. Tap **Add to Home Screen**
4. Tap **Add** — it will appear on your home screen with its own icon

### On Android
1. Open the URL in **Chrome**
2. Tap the three-dot menu → **Add to Home screen**

### On desktop
Just bookmark the URL — it works in any modern browser.

---

## Logging periods

Tap any day on the calendar to open a menu. From there you can:

- **Log that day as a period start**
- **Set it as the end of an open period**
- **Update the end date** of a nearby logged period
- **Remove** a period entry

You can also log periods manually in the **Log** tab.

---

## Syncing between devices (optional)

By default, data is stored only in your browser on that device. To sync across devices:

1. Go to the **Settings** tab
2. Tap **Sign in with Google**
3. Sign in with a Google account

Your data will be saved to a file called `cycle-tracker-data.json` in **your own Google Drive** — not anyone else's. Both devices merge data when they connect, so nothing is lost.

> **Note:** The Google sign-in uses your own Google account and goes directly to Google's servers. The app developer has no access to your Drive or your data.

---

## Privacy

This app was built with privacy as a core principle. Here is exactly what happens with your data:

- **No server.** There is no backend. The app is a single HTML file served by GitHub Pages.
- **No analytics.** There is no tracking, no cookies, no telemetry of any kind.
- **No account required.** You never sign up for anything to use this app.
- **Data stays on your device.** Period data is stored in your browser's `localStorage` — the same place a website might remember your preferences. It never leaves your device unless you choose to enable Google Drive sync.
- **Google Drive sync is optional and goes to your account.** If you use it, your data is saved to a JSON file in *your* Google Drive. The app developer cannot see it, access it, or retrieve it.
- **The code is fully open source.** Every line of code is visible at [github.com/rharder/cycle-tracker](https://github.com/rharder/cycle-tracker). You can verify exactly what the app does.
- **You can export your data at any time.** Go to Settings → Export JSON to download a backup of everything.

### A note about sensitive data

Period tracking data is personal health information. We take that seriously. Because this app has no server and no accounts, there is simply no mechanism by which your data could be collected, sold, or breached through this app. The only copies of your data are the ones you choose to keep.

---

## Backing up your data

Go to **Settings → Export JSON** to download a backup file. You can re-import it on any device using **Settings → Import JSON**.

---

## About

Built by [rharder](https://github.com/rharder). Free to use, free to fork.

If you find a bug or have a suggestion, open an [issue](https://github.com/rharder/cycle-tracker/issues).
