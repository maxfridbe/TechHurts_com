# techhurts.com

The TechHurts website. Static HTML and CSS, deployed to GitHub Pages by Actions.

> We make software. You install software. We charge you nothing. And you don't complain.

## No JavaScript, no Node.js

There is no build step, no bundler, no package manager and no `package.json`.
Every page is hand-written HTML that links one stylesheet. Graphics are inline
SVG, so the site loads nothing from any other host — no CDN, no web fonts, no
analytics. Editing a page means editing that page.

Keep it that way. If a change seems to need JavaScript, it probably needs a
different design instead.

## Files

| File | What it is |
|---|---|
| `index.html` | Home: the four ideals, privacy summary, TTS Runner spotlight, Vibecoding |
| `software.html` | The software library |
| `vibecoding.html` | AndroidBase and GameBase — the base repos the software is built on |
| `ttsrunner.html` | TTS Runner for Android — detail page, screenshots, permissions table, benchmarks |
| `img/` | Screenshots, committed to this repo and served from this domain |
| `privacy.html` | No telemetry, no background network use, no unexplained permissions |
| `manifesto.html` | The four sentences, at length |
| `tokens.html` | The crypto tip jar |
| `404.html` | Not-found page (uses absolute paths, since it is served from any URL) |
| `style.css` | The entire design |
| `CNAME` | Custom domain for GitHub Pages |
| `.nojekyll` | Serve files as-is |
| `.github/workflows/pages.yml` | Upload the repo root as a Pages artifact and deploy |

## Screenshots

`img/*.png` are the TTS Runner screenshots, downscaled to 360px wide from the
1080x2340 originals in the app repo (`docs/screenshots/`) so the pages stay
light. They are committed here rather than hot-linked, because loading images
from another host would contradict the privacy page. To refresh them:

```sh
ffmpeg -y -i <source>.png -vf "scale=360:-1:flags=lanczos" img/ttsrunner-<name>.png
```

Every `<img>` carries explicit `width`/`height` so the page doesn't reflow as
images load, and alt text describing what the screen shows.

## Publishing the tip-jar addresses

`tokens.html` ships with placeholders. When the wallets exist:

1. Replace `address-not-published-yet` in each `<div class="addr pending">` with
   the real address.
2. Remove `pending` from that div's class, so it stops rendering with the red
   hatched "not live" background.
3. Delete the rows for any chain you don't want to accept.
4. Delete the yellow "Addresses not published yet" `<div class="notice">` above
   them once at least one address is live.

Everything to change is inside the block marked `TIP ADDRESSES` in that file.

## Deploying

Push to `main`. The workflow uploads the repository root and publishes it.

One-time setup in the repository settings:

- **Settings → Pages → Build and deployment → Source:** *GitHub Actions*.
- **Settings → Pages → Custom domain:** `techhurts.com`, then tick
  *Enforce HTTPS* once the certificate is issued (it can take a few minutes).

DNS for the apex domain — four `A` records and four `AAAA` records at your
registrar:

```
A     @   185.199.108.153
A     @   185.199.109.153
A     @   185.199.110.153
A     @   185.199.111.153
AAAA  @   2606:50c0:8000::153
AAAA  @   2606:50c0:8001::153
AAAA  @   2606:50c0:8002::153
AAAA  @   2606:50c0:8003::153
CNAME www maxfridbe.github.io.
```

## Previewing locally

Open `index.html` in a browser. That is the whole toolchain.
