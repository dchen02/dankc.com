# Version 3 — more images + Instagram video embeds

Based on v2 (`../index.html`, unchanged). Changes in this folder only:

## Image fix (6 Nov 2026)
- Root cause: all `https://danchen.me/assets/*.jpg` links were 404 on the live site (checked tsp2.jpg, chapter2.jpg, chapter3.jpg, danchen.jpg — all 404; the resume PDF at the same path does load). The live host isn’t serving those JPGs, even though the files exist in Dan’s local folder.
- Fix: the actual JPGs/PDF were copied from `/Users/danchen/Documents/dankc.com/assets/` into `v3/assets/` and the page now uses relative paths (`assets/tsp2.jpg`, `assets/img/chapter2.jpg` … `chapter7.jpg`, `assets/img/danchen.jpg`, `assets/dan_chen_resume.pdf`). All 9 local files verified 200 locally. The site is now self-contained — deploy the whole folder and the images travel with it.
- OG / Twitter / JSON-LD image URLs stay absolute (`https://danchen.me/assets/tsp2.jpg`) on purpose: they need an absolute URL, and that path will work once this folder is deployed, because the file is now bundled.
- Instagram thumbnails are still remote CDN links and can expire / be blocked — they now hide themselves on error (`onerror`) instead of showing a broken-image icon, and the reel links always remain.

## Removed
- Every “in public” — hero lede, OG/Twitter descriptions, and the “Vibe-coding robots, in public” heading. Now just “prototyped fast” / “Vibe-coding robots”.

## More images
- Hero: `https://danchen.me/assets/tsp2.jpg` (the old site’s hero poster image).
- Selected Work: each of the 6 cards now has a photo from the original site, mapped by chapter:
  - Meta Ray-Ban — `assets/img/chapter2.jpg`
  - Culture Biosciences — `chapter3.jpg`
  - Team Spirit Generator — `chapter4.jpg`
  - Intimacy Machine — `chapter5.jpg`
  - End of Life Care Machine — `chapter6.jpg`
  - Cardboard CNC — `chapter7.jpg`
- About: portrait `assets/img/danchen.jpg`.
- Latest Work: grid expanded from 9 to all 12 reels, with thumbnails now in the HTML itself (not only added by JS). Thumbnails are Instagram CDN links from 6 Nov 2026 and can expire — reel links are durable.

## Instagram video embeds — yes, possible
- 3 featured reels are embedded as real video players using Instagram’s official embed:
  `https://www.instagram.com/reel/<code>/embed/`
  - 5 circular displays, 3D effect — Dek00bJxYCi
  - IKEA planter lamp — DfI34K4RI0l
  - 5 round displays, 30mm apart — DehLSHgxhcw
- Caveats: embeds load Instagram’s player, so they’re heavier than thumbnails and need Instagram to be reachable — they can show a login prompt / fail if Instagram blocks the viewer. Every embed has a reel link underneath as a fallback. A plain text fetch of the embed URL returned 403 (Instagram blocks bots), which is expected — it still plays in a normal browser. Check it in your browser when you preview.
- If embeds feel too heavy / flaky, the thumbnail grid alone is the safer version — that’s v2.

## Monochrome theme — gray, black, white, sharp corners (21 Nov 2026)
- `styles.css` only: palette is now white / black / grays only (no warm off-white, no orange accent), and every `border-radius` is `0` — no pill buttons, no rounded cards, images, or embeds. Same layout and type as before, just the sharper monochrome look Dan asked for.

## Embeds only — thumbnail grid removed (21 Nov 2026)
- The 12-card thumbnail grid is gone from `index.html`, along with the script that filled it from `ig-feed.json` and the `.ig-grid` / `.ig-card` CSS. Why: the Instagram CDN thumbnail links are signed URLs that expire — checked 21 Nov, they return 403 — so 11 of 12 cards showed blank. The `onerror` hide made it look like empty cards instead of broken images.
- Latest Work is now just the featured Instagram embeds (tower, lamp, OLED glasses — the top 3 by Views). Embeds are served live by Instagram, so they don't expire.
- 21 Nov 2026: 3 more embeds added — the next 3 by lifetime Views from the Nov 20 ranking: 16 lasers on 16 servos (DeNsgQYxhkX, 30k), motorized slider (DcdRCWthAba, 24k), how the transparent OLED glasses work (DcMdPNZh_bH, 10k). The section now shows 6 embeds in a 3×2 grid.
- Embed iframes are taller so the card doesn't inner-scroll: 720px desktop, 700px mobile (was 520px / 560px).
- Both fineprint notes under Latest Work are removed ("Videos above play inside Instagram's own embed player…" and "Best-performing reels, ranked by lifetime Views…"). The footer fineprint stays.
- `ig-feed.json` stays in the repo as the data record, but nothing on the page reads it anymore.

## Hero lede removed, professional line added (21 Nov 2026)
- The first-person hero lede ("I'm Dan. I build small machines… Day job: Meta…") is out of `index.html` — Dan felt it read as not professional enough for job applications. In its place, under the headline: "AI Product Designer at Meta, working on Ray-Ban smart glasses. 13 years across hardware, software, and connected experiences." (same `.lede` style, new copy).

## Best-performing feed + less AI-sounding copy (20 Nov 2026)
- Latest Work is now the 12 best-performing reels, ranked by lifetime Views (from the Nov 16 year review + Nov 20 fresh pull), not the 12 latest. Top 3: tower Dek00bJxYCi 1.32M, OLED glasses DcJ8duJB5ZU 231k, lamp DfI34K4RI0l 35k. Cutoff check ran before ranking: wearables sync DecBt73x6Du (5,963) and Porsche DfI4gADxjYp (4,076) made the cut; the Nov 19 key reel (3,426) and tower-detail Dek1c2BRZtg (3,666) did not.
- The 3 featured embeds are now the top 3 by Views (tower, lamp, OLED glasses) — the tower first-cut DehLSHgxhcw (3 likes) is out of the embeds and out of the grid.
- Each grid card shows a compact Views label (1.3M, 231k, …). `ig-feed.json` carries exact lifetime figures (top-8 figures from the Nov 16 pull, recent posts from the Nov 20 pull) and was regenerated by `/tmp/build_best_feed.py`.
- Copy pass to sound less AI-generated: hero lede is now first-person and specific ("I build small machines that do something strange and specific…"); the About section is a short first-person paragraph instead of CV lines, and the "Core skills" keyword list is gone; "Let's connect" → "Say hi"; Ray-Ban and Culture Biosciences cards rewritten in first person. The positioning line ("I design how people and intelligent machines understand each other") is untouched, as are the Team Spirit / Intimacy / End of Life / Cardboard CNC cards, which were already in Dan's voice.
- The "Now" tower link now points to the hero tower reel Dek00bJxYCi, not the first cut.

## Preview
```sh
cd ~/workspace/dankc.com/v3
python3 -m http.server 8001
# open http://localhost:8001/
```
