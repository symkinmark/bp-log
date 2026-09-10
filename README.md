# BP log

A 7-day blood pressure and pulse diary you can open on your phone instead of filling in the paper sheet. Three sessions a day (morning fasting, two hours after lunch, before dinner), three readings per session, then a report you can print or hand to your doctor.

**Your data never leaves your device.** There is no server, no account, no analytics and no cookies. Everything is saved in the browser's local storage on the device you use. The app itself can be hosted publicly; each person's readings stay private to their own browser.

## What it does

- Logs systolic, diastolic and pulse for 3 readings x 3 sessions x 7 days, with time and notes, in a layout that mirrors the paper form.
- Remembers everything on the device, opens on today's day automatically, fills in the time when you type the first reading, and pre-fills your name, arm and monitor when you start a new week.
- Keeps as many weeks as you like.
- Report tab: session and overall averages, a trend chart, and the full table. Print it (or save as PDF) for your appointment. Optional toggle to leave out the first reading of each session when averaging, if your doctor asks for that.
- Backup tab: download a JSON backup, restore one on another device, export the week as CSV, delete a week, or erase everything.
- Flags values that are outside what a home monitor normally shows (likely typos) and shows a rest-and-remeasure notice for very high readings. It does not interpret your numbers; that is what the appointment is for.
- Installable as an app (Add to Home Screen) and works offline.

## Deploy

The whole app is one static folder with no build step, so hosting is one click.

### Vercel

1. Push this folder to a GitHub repository.
2. On [vercel.com](https://vercel.com), choose **Add New > Project**, import the repository.
3. Leave **Framework Preset** as *Other*, no build command, no output directory. Click **Deploy**.

Or from your machine: `npx vercel` inside the folder and accept the defaults.

### GitHub Pages

1. Push this folder to a GitHub repository.
2. Repository **Settings > Pages**. Under *Build and deployment*, choose **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
3. After a minute the app is live at `https://<your-username>.github.io/<repo-name>/`.

All paths are relative, so it works at a sub-path like the one GitHub Pages uses.

### Run locally

Open `index.html` directly in a browser, or serve the folder with `npx serve` (the service worker for offline use only registers over http/https).

### Install on a phone

Open the deployed URL, then **Share > Add to Home Screen** (iOS) or the browser's **Install app** prompt (Android). It opens full-screen like a native app and works without a connection.

## Privacy model, plainly

- Storage is `localStorage` under the key `bplog.v1`. Nothing is sent anywhere.
- Different devices do not sync. Use **Backup > Download backup** and **Restore backup** to move your log.
- Clearing browser data, private-browsing mode, or uninstalling the home-screen app removes the log. Keep a backup before you clear anything.
- The hosted files are the same for everyone; they contain no data. Your readings only ever exist in your browser.

If you later want sign-in and syncing between devices, that needs a backend (for example Next.js on Vercel with an auth provider and a database). This version deliberately avoids that so there is nothing to secure and nothing to leak.

## Customise

Everything lives in `index.html`. The session names, meal conditions and the hour ranges used to highlight the current session are in the `SESSIONS` array near the top of the script. Ranges used to flag likely typos are in `LIMITS`.

When you change the app after it has been installed on a phone, bump `CACHE` in `sw.js` so the service worker refreshes the cached shell.

## Files

```
index.html             the app (HTML, CSS and JS in one file)
sw.js                  service worker for offline use
manifest.webmanifest   makes it installable
icons/                 app icons
```

MIT licensed. Fill in your name in `LICENSE`.
