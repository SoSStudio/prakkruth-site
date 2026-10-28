# prakkruth.com — deploy guide (28 Oct 2026)

Static site. No build step, no framework, no server code. Everything in this folder goes to the repo root.

## What's here
- `index.html` — the whole site (Index, Work, About, Contact). Pages switch with a wipe; URLs are `/#work`, `/#about`, `/#contact`.
- `media/` — card loops (+ poster stills), home background loop, reel, portrait, VP mark, CV PDF.
- `assets/fonts/` — self-hosted Unbounded, Space Mono, Hanken Grotesk (Unbounded patched so the outline name renders cleanly).
- `favicon.png`, `apple-touch-icon.png`, `og.jpg` (link-share preview), `robots.txt`, `sitemap.xml`, `404.html`, `CNAME`.

Total ≈ 13 MB. Largest file: `media/reel.mp4` (9.6 MB), well under GitHub's limits.

## Deploy (same way as southofsomewhere.studio)
1. Create a repo (e.g. `prakkruth-site`) and commit the contents of this folder at the root.
2. Connect it to the same host used for SoS and deploy the default branch. No build command; publish directory is the repo root.
   - GitHub Pages: Settings → Pages → Deploy from branch → `main` / root. `CNAME` is already included.
   - Vercel / Netlify / Cloudflare Pages: framework "None" / "Other", build command empty, output directory `/`. Delete `CNAME` if the host complains (harmless otherwise).
3. Add the custom domain `prakkruth.com` (and `www.prakkruth.com` → redirect to apex) in the host's dashboard.

## DNS at Hostinger — read before touching anything
Email for v@prakkruth.com is routed through **iCloud** via DNS records on this domain.
- **Only change the website records:** the apex `A` / `ALIAS` record(s) for `@` and the `CNAME` for `www`, using the values the host gives you.
- **Do NOT delete or edit:** `MX` records (iCloud), the `TXT` records for SPF (`v=spf1 include:icloud.com ...`) and Apple domain verification (`apple-domain=...`), and the DKIM `CNAME` (`sig1._domainkey`).
- Remove any leftover records pointing at the old Hostinger Website Builder (old `A`/`CNAME` for `@`/`www`) only after the new ones are added.
- Screenshot the full DNS zone before changing anything.

## After it's live — 5-minute check
- [ ] https://prakkruth.com loads over HTTPS; `www` redirects to the apex.
- [ ] Home background loop plays; name outline looks clean; hovering the name swaps outline/solid.
- [ ] Work: all 6 cards play their loop; tap to flip; **Watch** opens the in-page player (Vimeo showcases for Feature Films and Metro & Mochi; YouTube for myAIcademy and Retrograde); "Visit the studio" and "Visit the channel" open new tabs.
- [ ] Reel plays with sound; Jump-to chips seek to 0:00 / 0:16 / 0:33.
- [ ] About: Download CV gets the PDF.
- [ ] Contact: Write to me opens mail; LinkedIn and Instagram open the right profiles.
- [ ] Phone (Safari + Chrome): cards stack, loops play, nothing scrolls sideways.
- [ ] **Send a test email to v@prakkruth.com from another account and confirm it arrives.**
- [ ] Paste the URL into WhatsApp/LinkedIn and confirm the preview card shows the name image.

## Updating later
Edit `index.html` → the `CARDS` list near the bottom holds every card (title, line, loop file, link). Swap a loop by replacing the file in `media/` with the same name (keep it ~6 s, ≤1 MB, H.264 MP4, no audio) and regenerating its `.jpg` poster.
