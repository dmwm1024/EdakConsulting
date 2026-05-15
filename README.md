# Edak Consulting — edakconsulting.com

One-page static site for Edak Consulting, hosted on GitHub Pages at
[edakconsulting.com](https://edakconsulting.com).

## Repo layout

```
index.html        ← the live site (production build, self-contained)
CNAME             ← custom-domain pointer (edakconsulting.com)
.nojekyll         ← tells Pages: serve files as-is, don't run Jekyll
robots.txt
sitemap.xml
favicon.ico       ← (optional — site currently uses an inline SVG favicon)

design/           ← design-time sources, NOT served to visitors directly
├── design.html      ← Tweakable design version (live theme/accent/copy controls)
├── editorial.html   ← earlier editorial-direction explore
├── image-slot.js    ← web component used only by design.html
└── tweaks-panel.jsx ← React tweaks panel used only by design.html
```

`index.html` is fully self-contained — it inlines all CSS, loads only Google
Fonts from the network, and has no references to anything in `design/`.
`design/` is kept in the repo as design history; GH Pages will serve those
files too if anyone requests them by URL, but nothing links to them.

## One-time setup

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin git@github.com:dmwm1024/EdakConsulting.git
git push -u origin main
```

### 2. Enable GitHub Pages

In the repo on github.com:

1. **Settings → Pages**
2. Under **Build and deployment → Source**, choose **Deploy from a branch**
3. **Branch:** `main`, **Folder:** `/ (root)`
4. Click **Save**

GitHub will publish the site at `https://dmwm1024.github.io/EdakConsulting/`
within a minute or two. Confirm it loads there before pointing the domain.

### 3. Configure the custom domain (edakconsulting.com)

The `CNAME` file already tells Pages that the canonical hostname is
`edakconsulting.com`. You also need DNS records at your domain registrar.

At your registrar (Namecheap, Cloudflare, GoDaddy, etc.) add these records:

| Type  | Host / Name | Value                       |
|-------|-------------|-----------------------------|
| A     | @           | `185.199.108.153`           |
| A     | @           | `185.199.109.153`           |
| A     | @           | `185.199.110.153`           |
| A     | @           | `185.199.111.153`           |
| CNAME | www         | `dmwm1024.github.io.`       |

Then back in **Settings → Pages**:

1. **Custom domain:** enter `edakconsulting.com` and Save
2. Wait for the green DNS check (usually 5–30 minutes)
3. Tick **Enforce HTTPS** once it becomes available (Pages provisions a
   Let's Encrypt cert automatically — can take up to 24 hours)

Edits to `index.html` go live within ~1 minute of pushing to `main`.

## Editing copy

`index.html` is one file. Search for the text you want to change:

- Headline: search `When your business needs`
- Service blurbs: under `<!-- ─── SERVICES ─── -->`
- Process steps: under `<!-- ─── HOW IT WORKS ─── -->`
- About bio: under `<!-- ─── ABOUT ─── -->`
- Calendly link appears in three places — search
  `calendly.com/kade-edakconsulting` to update all of them at once.

## Adding a real portrait

The About section shows a `KM` monogram placeholder. To use a real photo:

1. Drop the photo into `assets/kade.jpg` (4:5 ratio works best — e.g. 800×1000).
2. In `index.html`, find this block:

   ```html
   <div class="portrait-mono" aria-hidden="true">
     <span class="portrait-mono-letters">KM</span>
     <span class="portrait-mono-label">PORTRAIT</span>
   </div>
   ```

   and replace it with:

   ```html
   <img class="portrait-img" src="assets/kade.jpg" alt="Kade McAlpin" />
   ```

The `.portrait-img` CSS is already in place — it'll fill the frame with
`object-fit: cover`.

## Iterating on the design

`design/design.html` is the same site but wired up with the live Tweaks panel
(theme, accent color, headline emphasis, headline copy). Open it locally to
play with variations. When you're happy with a change, copy the relevant
CSS / markup over into the root `index.html` to ship it.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Or double-click `index.html` — `file://` works too.
