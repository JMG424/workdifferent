# Work Different — editorial site

The site for the "Work Different" series and studio. Designed to feel like the reels themselves: editorial, dense, monospace-dominant, with serif italic reserved for the wordmark and innovator names. Cream on near-black. Warm accent used as section markers, not decoration.

## What it is

This is not a services landing page. It's an editorial magazine front page that happens to also offer services at the end. The innovators are the brand. The studio is what powers the brand.

## File structure

```
site/
├── index.html              # the whole page, self-contained
├── portraits/              # 4 cleaned B&W innovator portraits (3:4 aspect)
│   ├── tesla.jpg
│   ├── curie.jpg
│   ├── wright.jpg
│   └── davinci.jpg
└── README.md
```

Total weight: ~1.1MB.

## What's on the page

1. **Hero** — wordmark + lede + rotating featured portrait, with editorial meta strip ("5 profiled · 40+ in queue · weekly drop")
2. **The Series** — a vertical stack of 4 editorial cards, one per innovator, each formatted the same way as the reels: portrait + name + dates + // THE STORY / // WHAT THEY BUILT / // WHY IT MATTERS NOW
3. **Manifesto** — serif italic, "to the ones who..." standing as the studio's stake in the ground
4. **The Thesis** — three pillars explaining what the studio believes about work
5. **Closing** — "Your turn." + Work-with-us CTA + Subscribe secondary

## Deploy

Static site. Drag into Cloudflare Pages / Netlify Drop, or push to GitHub Pages.

Already on GitHub Pages at: `https://jmg424.github.io/workdifferent/`

To update:
```bash
unzip -o ~/Downloads/workdifferent_site.zip -d /tmp/
cp -r /tmp/site/* ~/Desktop/site/    # adjust path to your local repo
cd ~/Desktop/site
git add -A
git commit -m "v5: editorial redesign matching the reels"
git push
```

## Wire up the live endpoints

Two placeholders before going live:

1. **"Work with us"** button — replace `href="#"` on `#work-with-us` with your Calendly/Cal.com URL
2. **Subscribe form** — replace the `alert()` in the modal handler with a real Kit/Mailchimp/Formspree fetch

Both are marked `TODO` in the source.

## Adding a new innovator

1. Save the public-domain portrait as `portraits/<name>.jpg` (the build script in the older zip handles B&W conversion + 3:4 crop)
2. In `index.html`, copy an existing `<article class="card">` block and update:
   - `<img src="portraits/<name>.jpg">`
   - `.badge` number
   - `.card-eyebrow` profile number
   - `<h3>` name and `.dates`
   - The three `.body` lines under `THE STORY` / `WHAT THEY BUILT` / `WHY IT MATTERS NOW`
3. Also add to the hero rotation: append a new `<img>` to the `.hero-portrait` block

## Design notes

- **Type**: Fraunces serif italic for the wordmark + innovator names; JetBrains Mono for everything else; Inter as a subtle fallback (not currently used but referenced for future expansion).
- **Color**: Near-black `#0a0a0c`, cream `#ebe5d4` (matches the reels' body text), warm tan `#d9a76b` (matches the section labels), hairline grey for dividers.
- **Voice**: `// SECTION LABEL` everywhere a label appears. Section labels are always uppercase mono with the `//` prefix. This is the brand's visual signature.
- **No emoji. No decoration. No floating accent dots that don't earn their keep.**
