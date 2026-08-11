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
| _(no `CNAME`)_ | Custom domain is **off** — see "Custom domain" below |
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

## Custom domain (techhurts.com) — currently disabled

The `CNAME` file was removed on purpose. GitHub honours it as soon as it is
committed and starts 301-redirecting the `github.io` URL to the custom domain —
so while `techhurts.com` does not resolve to GitHub, committing it leaves the
site with **no reachable URL at all**. Re-add it only once DNS is live:

```sh
echo techhurts.com > CNAME && git add CNAME && git commit -m "Enable custom domain" && git push
```

DNS for the apex — four `A` records and four `AAAA` records:

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

The apex **must** be A/AAAA records. A `CNAME` on the apex is invalid — the
zone already has SOA and NS records there — and DNS providers reject or
silently drop it. Only `www` may be a CNAME.

Verify from the authoritative nameservers before re-adding the file:

```sh
getent ahosts techhurts.com     # must show 185.199.108-111.153
```

## Previewing locally

Open `index.html` in a browser. That is the whole toolchain.
