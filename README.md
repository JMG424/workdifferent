# Work Different — brand site

A pure-brand landing page for the Work Different studio. Innovators are the design. Services are not on this page.

## File structure

```
site/
├── index.html              # the entire page, self-contained
├── reels/                  # 8-second silent looping reels (~70KB each)
│   ├── edison.mp4
│   ├── tesla.mp4
│   ├── curie.mp4
│   ├── wright.mp4
│   └── davinci.mp4
└── posters/                # first-frame fallback images for instant load
    ├── edison.jpg
    ├── tesla.jpg
    ├── curie.jpg
    ├── wright.jpg
    └── davinci.jpg
```

Total weight: ~600KB. Loads in under a second on any connection.

## Deploy

The site is fully static. Drop the folder on anything that serves files:

- **Cloudflare Pages / Netlify drop**: drag the `site/` folder onto the dashboard. Done.
- **GitHub Pages**: push the folder contents to a repo, enable Pages. Done.
- **Your own VPS**: `scp -r site/ user@host:/var/www/workdifferent/` then point nginx/Caddy at it.

No build step. No dependencies. No package.json.

## Wire up the live endpoints

Two things in `index.html` are placeholders you'll want to replace before going live:

### 1. The "Work with us" button (closing CTA + nav-less mobile state)

Currently linked to `#`. Search for `id="work-with-us"` and replace `href="#"` with your Calendly / Cal.com / Tally URL.

### 2. The email capture form (Join the studio modal)

Currently shows an alert box on submit. Search for the comment `// TODO: replace with real form endpoint` and swap the alert for a real form action — Kit (ConvertKit), Mailchimp, Formspree, ConvertKit, or a simple fetch to your backend.

Example for Kit (ConvertKit):

```js
fetch('https://app.convertkit.com/forms/YOUR_FORM_ID/subscriptions', {
  method: 'POST',
  body: new URLSearchParams({ email_address: email }),
}).then(() => {
  modal.classList.remove('open');
  // show success state
});
```

## Adding a new innovator reel

1. Pre-trim a new reel to ~8 seconds, silent, 720p wide:
   ```bash
   ffmpeg -ss 14 -i source.mp4 -t 8 -an -vf "scale=720:-1" \
     -c:v libx264 -preset slow -crf 28 -movflags +faststart \
     -pix_fmt yuv420p reels/newperson.mp4
   ```

2. Generate a poster from the first frame:
   ```bash
   ffmpeg -i reels/newperson.mp4 -frames:v 1 -vf "scale=540:-1" \
     posters/newperson.jpg
   ```

3. Add a new `<div class="reel">` block inside the `<div class="reels">` in `index.html`. Copy any existing one and change `source.src`, `video.poster`, and the `.reel-label` text.

4. Update the count in the gallery header (`5 of 40` etc.).

## Notes on the design

- **Typography**: Fraunces (serif, italic for display) + JetBrains Mono (everything else). Both loaded from Google Fonts.
- **Color**: Near-black background (`#0a0a0c`), cream text (`#ece6d6`), single warm accent (`#d97a3c`). One color, used sparingly.
- **Motion**: Only on user actions and on initial reveal. Videos loop silently, marquee scrolls horizontally, page itself is otherwise still.
- **Film grain**: Applied as a fixed SVG overlay via `body::before`. Adds texture without visual noise. Adjust opacity in the CSS variable section if too strong.
