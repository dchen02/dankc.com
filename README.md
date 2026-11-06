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

## Preview
```sh
cd ~/workspace/dankc.com/v3
python3 -m http.server 8001
# open http://localhost:8001/
```
