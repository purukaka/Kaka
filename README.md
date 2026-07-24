# purukaka.com — deploy guide

## This version: dark cinematic design

`index.html` is now the **dark cinematic build** — near-black background, amber/gold accents, Bricolage Grotesque display type. It replaces the earlier cream/paper editorial design (still fine as a reference, but no longer the flagship).

**All photos are embedded directly in the HTML** (base64) — there is no `assets/photos/` folder to manage; the file is fully self-contained and portable.

- **Hero** — Nepal flag on a Himalayan summit portrait, cinematic amber grade, with three real facts in a credibility strip (Okay Pasal 2018, Kantipur Film Academy 2021–2023, Percept Media 2019).
- **3D motion** — a Three.js wireframe (icosahedron/torus/octahedron) drifts behind the hero text, layered with softly glowing amber "bubbles" rising through the scene, plus a subtle 3D tilt on cards, ventures, and gallery photos.
- **Touch-reactive headline** — the hero `<h1>` has a radial gradient glow that follows the pointer (mouse) or finger (touch) in real time.
- **Custom cursor + scroll progress bar** for a premium feel; both respect `prefers-reduced-motion` and are disabled on touch devices.
- **Courses, AI Tips & Prompts, Book a Call** — same content and functionality as before (category filters, copy-to-clipboard, Cal.com-ready booking with an honest fallback), restyled for the dark theme.
- **Gallery lightbox** — click any "Beyond the Work" photo to view it enlarged; click outside, ✕, or Escape to close.
- **Mobile nav** — hamburger menu; all motion effects gracefully disable on touch/small screens.

**Setup still needed:**
- **Book a Call** → replace `YOUR-CALCOM-USERNAME/consultation` in `index.html` with a real Cal.com link once you have one.
- **Contact info** → this build uses `directorpuru@gmail.com` / `@directorpuru` throughout (matching the existing blog at adhikaripurushottam.com.np). An earlier draft used `purukaka1111@gmail.com` / `@1purukaka` instead — confirm which is correct and update every mailto/social link in `index.html` if needed.

## File structure

Upload everything to your host's web root so it looks like this:

```
/
├── index.html                  ← FLAGSHIP — dark cinematic design, all photos embedded inline
├── index-editorial-backup.html ← previous flagship — cream/green editorial design, kept as a backup
│                                  (uses assets/photos/ — keep that folder if you keep this file)
├── index-v3-backup.html        ← earlier flagship (no Courses/AI Tips/Booking), kept as a backup
├── index-classic-backup.html   ← earliest text-only minimal version, kept as a backup
├── index-bold.html             ← alternate dark/violet/lime design (optional, not linked anywhere by default)
├── dashboard.html               ← staging CMS — keep this out of search engines (see note below)
├── CNAME                        ← contains "purukaka.com", required for GitHub Pages custom domain
├── favicon.ico
├── favicon.svg
├── apple-touch-icon.png
├── site.webmanifest
├── robots.txt
├── sitemap.xml
└── assets/
    ├── icon-192.png
    ├── icon-512.png
    ├── og-image.png            ← generated, matches the cinematic theme
    └── photos/                  ← only used by index-editorial-backup.html
```

## Before you go live — checklist

- [x] **Flagship (`index.html`) photos are in** — embedded directly in the HTML, nothing to wire up.
- [x] About timeline years are filled in (2018 / 2021–2023 / 2019).
- [x] Real blog links point to adhikaripurushottam.com.np.
- [x] `og-image.png` (1200×630px) generated in the cinematic theme.
- [x] `CNAME` file added for GitHub Pages custom domain.
- [x] `robots.txt` disallows `/dashboard.html`.
- [ ] **Confirm contact info.** `index.html` uses `directorpuru@gmail.com` / `@directorpuru`. If `purukaka1111@gmail.com` / `@1purukaka` is actually correct instead, update every mailto and social link in `index.html` (and the JSON-LD block near the top).
- [ ] **Book a Call** still needs a real Cal.com link — search `YOUR-CALCOM-USERNAME` in `index.html`.
- [ ] Update `sitemap.xml` if you add more pages later.

## Hosting options

### Option A — Netlify (easiest, free tier, good for a static site like this)
1. Go to [app.netlify.com](https://app.netlify.com) and sign up/log in.
2. Drag the whole project folder onto the "Deploy manually" area on your dashboard — no git required.
3. Once deployed, go to **Domain settings → Add a domain** and enter `purukaka.com`.
4. Netlify will give you DNS records (usually an A record or Netlify DNS nameservers). Add those at your domain registrar (wherever you bought purukaka.com).
5. Enable HTTPS (Netlify does this automatically via Let's Encrypt once DNS propagates).

### Option B — Vercel (similar to Netlify)
1. Go to [vercel.com](https://vercel.com), sign up, and choose "Add New Project."
2. Since this isn't a git repo, use the Vercel CLI instead: install with `npm i -g vercel`, then run `vercel` inside the project folder and follow the prompts.
3. Add `purukaka.com` under Project → Settings → Domains, and update DNS at your registrar as instructed.

### Option C — Traditional shared hosting / cPanel
1. Log into your host's cPanel (or equivalent) and open **File Manager**, or connect via FTP (FileZilla, etc.).
2. Navigate to `public_html/` (or your domain's document root).
3. Upload every file in this project, preserving the `assets/` folder structure.
4. Make sure `index.html` is directly in the root — that's what loads at `purukaka.com`.
5. If your host doesn't already point purukaka.com's DNS to it, update the domain's nameservers or A record to your host's values (found in your hosting welcome email or cPanel's "Domains" section).

### Option D — GitHub Pages
1. Create a new repository on GitHub (e.g. `purukaka-site`).
2. Push everything in this folder to that repo — the `CNAME` file (already included, containing `purukaka.com`) is what tells GitHub Pages your custom domain.
   ```
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/purukaka-site.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**, set the source branch to `main` (root), and save.
4. Under **Settings → Pages → Custom domain**, confirm `purukaka.com` shows up (it should auto-detect from the `CNAME` file) and enable **Enforce HTTPS** once it's available.
5. At your domain registrar, point DNS to GitHub Pages: add an `A` record for the apex domain pointing to GitHub's IPs (185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153), or a `CNAME` record if using a `www` subdomain instead. Full details: https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site


## Switching between the minimal and bold designs

Both `index.html` and `index-bold.html` share the same section IDs and content. To make the bold version live instead:
- Rename current `index.html` → `index-old.html` (backup)
- Rename `index-bold.html` → `index.html`
- Re-upload

## Updating content after launch

Open `dashboard.html` locally in a browser (no server needed — just double-click it), edit services/ventures/blog posts, then copy the generated HTML from each panel's export box into the matching `<!-- START -->` / `<!-- END -->` block in `index.html`. Re-upload `index.html` to your host to publish the change.

**Note:** the dashboard's generated HTML matches the text-only structure (title, description, sub-offerings) — it doesn't manage the flagship version's photos or icons. After pasting a dashboard export into a venture or service, add the corresponding `<img>` or icon manually using an existing entry as a template.
