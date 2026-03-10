# BodyShift — Deploy to iPhone via GitHub Pages

Turn BodyShift into an installable iPhone app using GitHub Pages. No Apple Developer account or App Store needed.

---

## What You'll Get

BodyShift runs as a **Progressive Web App (PWA)**. Once installed it looks and feels like a native iPhone app: its own icon on your home screen, full-screen display with no browser bar, offline support, and full camera access.

---

## Files to Upload

You should have these 7 files in your folder:

| File | Size | Purpose |
|------|------|---------|
| `index.html` | ~63 KB | The complete app |
| `manifest.json` | ~1 KB | Tells iOS to treat this as an installable app |
| `sw.js` | ~2 KB | Service worker for offline support and caching |
| `icon-192.png` | ~1 KB | App icon (Android/general) |
| `icon-512.png` | ~4 KB | High-res app icon |
| `apple-touch-icon.png` | ~1 KB | iPhone home screen icon (180x180) |
| `DEPLOY-TO-IPHONE.md` | — | This guide (don't need to upload, but can) |

---

## Step 1: Create a GitHub Account

Skip if you already have one. Go to **github.com** and sign up (free).

## Step 2: Create a New Repository

1. Click the **+** button (top-right corner) then **New repository**
2. Repository name: **bodyshift**
3. Set to **Public** (required for free GitHub Pages hosting)
4. Tick **"Add a README file"**
5. Click **Create repository**

## Step 3: Upload the App Files

1. In your new repository, click **Add file** then **Upload files**
2. Drag in ALL 6 app files: `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`
3. Commit message: `Add BodyShift app`
4. Click **Commit changes**

## Step 4: Enable GitHub Pages

1. Go to your repository **Settings** tab (the gear icon)
2. In the left sidebar, click **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Under **Branch**, select **main** and folder **/ (root)**
5. Click **Save**
6. Wait 2-3 minutes for the site to build
7. Refresh the Settings > Pages section — you'll see a green banner:
   **`https://YOUR-USERNAME.github.io/bodyshift/`**

## Step 5: Install on Your iPhone

1. Open **Safari** on your iPhone — this MUST be Safari, not Chrome
2. Go to your site: `https://YOUR-USERNAME.github.io/bodyshift/`
3. **Wait for the page to fully load** (important — don't rush this step)
4. Tap the **Share** button (square with an upward arrow, bottom of screen)
5. Scroll down and tap **"Add to Home Screen"**
6. Name it **BodyShift** and tap **Add**
7. The app now appears on your home screen

---

## Preventing the Blank Screen Issue

The most common problem with PWAs on iOS is a white/blank screen when opening from the home screen. This build has been specifically engineered to avoid it, but here's what to know:

**Why it happens (and how we've fixed it):**

1. **Wrong start_url paths** — Most PWA tutorials use absolute paths like `/index.html`, but on GitHub Pages your site lives at `/bodyshift/index.html`. Our manifest uses `./index.html` (relative), which works from any subdirectory automatically.

2. **Missing scope** — iOS requires an explicit `scope` in the manifest, or it opens links in Safari instead of the app. Ours is set to `./`.

3. **Service worker path mismatch** — The SW dynamically detects its own base path using `self.registration.scope`, so it caches the correct URLs regardless of where you host it.

4. **Stale cache** — Our SW uses "stale-while-revalidate" strategy: it serves cached content instantly but fetches fresh versions in the background, so updates arrive without blank screens.

**If you still get a blank screen:**

1. Open the app URL directly in Safari first (not from the home screen)
2. Wait for the page to fully load and the service worker to install
3. Then add to home screen
4. If that doesn't work: go to **iPhone Settings > Safari > Advanced > Website Data**, find your GitHub Pages domain, and tap **Delete**. Then revisit the site in Safari and re-add to home screen.
5. Make sure you don't have **Content Restrictions** enabled in **Settings > Screen Time > Content & Privacy Restrictions > Content Restrictions > Web Content**. Setting this to "Unrestricted" fixes many PWA issues.

---

## Important Notes

**Camera:** The first time you use the camera, Safari will ask for permission. Tap "Allow". The app uses your phone's rear camera by default — tap the flip button for the front camera.

**Privacy:** All photos and data are stored locally on your device using IndexedDB. Nothing is uploaded anywhere. Your transformation photos are completely private.

**Offline:** After the first visit, the app works offline. You can take photos and log weight without an internet connection.

**Storage:** iOS may clear PWA data if the app isn't used for several weeks. Use the **Export JSON** feature in Settings periodically to back up your data.

**Timer:** Use the countdown timer (3s, 5s, or 10s) to set your phone on a tripod, then walk into frame before the photo is taken.

---

## Updating the App

When you want to add features or make changes:

1. Edit `index.html` in your GitHub repository (click the file, then the pencil icon)
2. **Important:** Also edit `sw.js` and change `bodyshift-v2` to `bodyshift-v3` (or increment the number). This tells the service worker to refresh its cache.
3. Commit both changes
4. GitHub Pages redeploys automatically (takes 1-2 minutes)
5. On your iPhone, open and close the app — the new version loads in the background

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Blank/white screen from home icon | Clear Safari data for the domain, revisit in Safari, re-add to home screen |
| Camera shows black | Close + reopen the app. Check Settings > Safari > Camera is set to Allow |
| "Add to Home Screen" not showing | You must use Safari (not Chrome). Scroll down in the Share menu — it's below the first row of icons |
| Photos disappeared | iOS can clear PWA storage after weeks of inactivity. Export your data regularly via Settings > Export JSON |
| Ghost overlay not showing | You need at least one previous week's photos for the overlay to appear. On week 1, you'll see body outline guides instead |
| App opens in Safari (with browser bar) | The manifest scope may not match. Make sure all files are in the root of your repository, not in a subfolder |
| Timer not counting down | Tap the clock icon next to the shutter to select 3s/5s/10s first, then tap the shutter |

---

## Optional: Custom Domain

If you own a domain name:

1. In repository **Settings > Pages > Custom domain**, enter your domain
2. Add a CNAME record via your DNS provider pointing to `YOUR-USERNAME.github.io`
3. Tick **Enforce HTTPS**
4. The manifest's relative paths (`./`) mean it will work automatically on your custom domain too
