# 🐯 Tigerkarten

A German–English flashcard trainer that pairs **Anki's spaced‑repetition engine** with **Duolingo's game layer**. It ships with a hand‑reviewed **2,452‑word deck** across 18 topics. Progress is saved in your browser, and can sync across devices through a file in your own cloud folder — no accounts, no server.

No build step, no dependencies, no server — a single HTML page plus a data file.

![Tigerkarten](https://img.shields.io/badge/cards-2452-F5902B) ![No build](https://img.shields.io/badge/build-none-5AB81E) ![PWA](https://img.shields.io/badge/PWA-installable-1C9BE6)

## Features

**The Anki half — the engine**
- **SM‑2 spaced repetition**: every card carries an ease factor, interval and due date, with learning steps (1 min → 10 min → graduate). Grades give **distinct, strictly‑increasing intervals** (Again / Hard / Good / Easy are always different).
- **Flip cards** with `Again / Hard / Good / Easy` grading — each button shows its *real predicted interval* (`10m`, `2d`, `3d`, `4d` …).
- **Bounded daily review**: the home button gives you a capped batch (default **30**, adjustable) of the **hardest due cards**, so sessions don't balloon as you learn more.
- **Per‑category control**: tap any topic to choose **Wiederholen** (review everything due in it) or **Neu lernen** (introduce new words, within your daily new‑card budget).
- **Reversed cards** (optional): each word becomes two independently‑scheduled cards — DE→EN *and* EN→DE — exactly like Anki's reverse cards.
- **Add your own cards**, which land in a "My Cards" unit and schedule normally.

**The Duolingo half — the feel**
- **Streak, XP, gems, hearts and levels** in a sticky HUD.
- **New words are taught with 4‑option multiple choice** (distractors drawn from the same topic), then handed to the SRS engine.
- **Daily XP goal ring**, combo bonuses, floating +XP, confetti, sound effects.
- A winding **unit path** across the 18 topics with mastery crowns.
- **Hearts** deplete on wrong answers and refill over time (or with gems) — fully toggleable.

**Sync & reminders** (Settings ⚙)
- **Cross‑device progress, no accounts.** On a computer, pick a **sync file once** and the app saves to it automatically on every change. Put that file in a **Google Drive / Dropbox / iCloud** folder and it rides that service's own sync to your other devices.
- **Big Save button** on the home screen — saves to your sync file instantly (or downloads a backup on iPhone). Progress also **auto‑saves periodically**, and the app **warns before you leave** if there are unsaved changes.
- **Backup / Restore** — a one‑tap export/import that works on *every* device (including iPhone, where browsers block direct file access).
- **Daily reminder** — an optional notification when your goal is still open. Install to your home screen for the best chance of it firing.
- **Installable (PWA)** — add it to your home screen / desktop and it runs full‑screen and offline.

## Cross‑device progress — how it works

There are **no accounts and no setup**. The trick is to let a cloud folder you already have do the syncing:

1. Open **Settings → Geräteübergreifend speichern → “Sync‑Datei wählen.”**
2. Save the file (`tigerkarten-progress.json`) **inside your Google Drive / Dropbox / iCloud Drive folder.**
3. That's it — every change now writes to that file automatically, and your cloud app copies it to your other computers. Open the app there, pick **the same file**, and your progress loads.

**On iPhone / iPad / Safari / Firefox**, browsers don't allow that automatic file access. Use **Backup** to download your progress and **Restore** to load it on the other device (e.g. AirDrop or email the file to yourself). It's manual, but it's the honest limit of what a no‑server web app can do there.

> Sync is **last‑writer‑wins** — fine for one person across devices; don't study on two devices at the same second and expect a merge.

## Reminders — the honest version

A website **cannot reliably push a notification when it's fully closed** without a paid push server — this is a browser limitation, worst on iPhone. So Tigerkarten's reminder is **best‑effort**:

- Turn it on in **Settings → Erinnerung**, grant notification permission, and pick a time.
- **Install the app to your home screen** (PWA) for the best reliability.
- It reliably nudges you when the app is open past your reminder time with your goal unmet, and when you re‑open it. It may **not** fire while fully closed, especially on iOS.

For a guaranteed daily reminder you'd need a push backend (e.g. Firebase Cloud Messaging) — out of scope for a zero‑server app, but easy to add later if you want it.

## Run it locally

Because the page loads `cards.js` as a sibling file, open it through a tiny local server rather than `file://`:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

Or use any static server you like (`npx serve`, VS Code Live Server, etc.).

## Publish it on GitHub Pages

1. Create a new repository and push these files to it:
   ```bash
   git init
   git add .
   git commit -m "Tigerkarten"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
2. On GitHub: **Settings → Pages → Build and deployment → Deploy from a branch**, pick **`main`** and folder **`/ (root)`**, then **Save**.
3. Your app goes live at `https://<you>.github.io/<repo>/` within a minute or two.

`index.html` is the entry point, so Pages serves it automatically — no configuration needed.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The entire app — UI, styles, SRS engine, game logic, sync + reminders |
| `cards.js` | The 2,452‑word deck as `window.CARD_DATA` (a compact array) |
| `manifest.json` | PWA manifest (name, colors, icon) so the app is installable |
| `sw.js` | Service worker — offline caching + reminder click handling |
| `icon.svg` | App / notification / favicon tiger icon |
| `LICENSE` | MIT |

### Card data shape

`cards.js` defines one global array; each row is:

```js
["monkey","Affe","der","noun","people and professions",2,"Der Affe sitzt im Baum…","The monkey sits in the tree…"]
// [ english, german, article, part-of-speech, category, difficulty, example_de, example_en ]
```

To use your own vocabulary, replace `cards.js` with the same structure. Empty strings are fine for the article/example fields.

## Notes

- Progress lives in your browser's `localStorage` (key `tigerkarten.v1`) as the always‑on cache; the sync file and Backup/Restore are how it moves between devices.
- **Sync, reminders and install need HTTPS.** GitHub Pages serves HTTPS automatically; `localhost` also counts as secure. Plain `file://` won't enable them.
- The in‑Claude artifact preview runs in a sandbox, so **sync and notifications are inactive there** — they light up on the deployed GitHub Pages site.
- The interface language is German, fitting for a German trainer.

## License

MIT — see [LICENSE](LICENSE).
