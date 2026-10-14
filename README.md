# Holding Trainer

A small web app for practising IFR holding entries. It runs entirely in the browser, needs no app store, and works offline after the first load on both iOS and Android.

## Features

- **Quiz:** You get the course to the fix and the holding instructions (inbound course, turn direction) and pick the entry: direct, teardrop or parallel. After answering, the app shows a map with the sectors and the entry path, plus a short description of the procedure.
- **Check for entry heading (optional):** Tick the checkbox below the quiz to also set a heading bug to the first heading you would fly after crossing the fix. Select the entry, set the bug (±5° tolerance) and tap **Check answer**. Both parts count towards the score, and the feedback tells you which one was wrong.
- **Custom:** Set the course to the fix, the inbound course and the turn direction (right or left) yourself. Entry, instrument and map update live. The required entry heading after the fix is shown as a number, as a heading bug on the instrument and as an arrow on the map.
- **Instrument:** A heading card with an integrated HSI. The course pointer is set to the inbound course, the deviation bar moves according to your position relative to the inbound line (5° per dot, 10° full scale), plus a TO/FROM indicator. Fixed white marks at ±90° from the nose make it easier to read relative courses.
- **Score:** Correct answers and streak are stored locally in the browser.

## Entry logic

The app uses the standard sector layout with the 70° line through the fix. The reference is the course with which you reach the fix.

| Entry    | Sector | Procedure (right turns) |
|----------|--------|-------------------------|
| Direct   | 180°   | Turn right at the fix onto the outbound heading and fly outbound |
| Teardrop | 70°    | Turn onto a heading 30° off the outbound heading towards the holding side, fly 1 minute, then turn right and intercept the inbound course |
| Parallel | 110°   | Turn onto the outbound heading, fly 1 minute, then turn left and intercept the inbound course from the holding side |

The entry heading is the first heading flown after crossing the fix: the outbound heading (inbound course + 180°) for direct and parallel entries, and the outbound heading offset by 30° towards the holding side for teardrop entries.

For left-hand holdings everything is mirrored. Wind and fix-passage details are not modelled.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole app (HTML, CSS and JavaScript in one file) |
| `sw.js` | Service worker that caches the app for offline use |
| `manifest.webmanifest` | Metadata for installing the app (name, icons, standalone mode) |
| `icon-180.png` | Home screen icon for iOS |
| `icon-192.png`, `icon-512.png` | App icons for Android (required for installation) |

Keep all files in the same folder, ideally the root of the repository.

## Publish with GitHub Pages

1. Create a new public repository, for example `holding-trainer`.
2. Upload all files (`index.html`, `sw.js`, `manifest.webmanifest`, the three icons and `README.md`) via **Add file → Upload files** and commit.
3. Go to **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute or two the app is available at `https://<username>.github.io/holding-trainer/`.

GitHub Pages serves the site over HTTPS, which is required for the service worker and for installation.

## Install and use offline

Open the app URL once while online, then install it:

### iPhone / iPad (Safari)

1. Open the URL in **Safari** (other iOS browsers may not offer installation).
2. Tap **Share**, then **Add to Home Screen**, then **Add**.
3. Start it from the home screen. It opens full screen and works without a connection.

### Android (Chrome)

1. Open the URL in **Chrome**.
2. Tap the menu (⋮) and choose **Install app** (or **Add to Home screen**).
3. Start it from the home screen or the app drawer. It opens in its own window and works without a connection.

Other Android browsers (Samsung Internet, Edge, Firefox) offer a similar **Add to Home screen** / **Install** entry in their menus.

### Notes

- Installed web apps appear as normal app icons. Home screen widgets are not possible for web apps on either platform.
- On iOS, data stored by a home-screen web app is kept separately from Safari, so the score starts fresh after installing.
- The first load must happen online so the service worker can cache the files.

## Releasing updates

After changing `index.html`, increase the version in `sw.js` (for example `holding-trainer-v3` to `holding-trainer-v4`). Otherwise devices keep serving the cached old version. You may need to open the app twice until the new version is active.

## Disclaimer

This app is for training and illustration only. It does not replace official documents or proper training and is not intended for use in flight. The applicable procedures and regulations always take precedence.
