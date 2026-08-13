# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

The product site + documentation handbook for **Maestro**, the immersive experience
authoring suite for Apple Vision Pro. Lives at the domain **maestrostud.io**; served
locally by Laravel Herd at **http://maestrostudio.test**. Migrated 2026-08-12 from the
retired chapter.vision site (repo `~/Herd/chaptervision`, kept for history).

Pure static HTML/CSS/JS — **no build step, no framework, no package.json**. The only
external dependency is Google Fonts (Fraunces, Outfit, JetBrains Mono). Deploys to
GitHub Pages from `main` root; `CNAME` pins maestrostud.io, `.nojekyll` protects
`docs/_TEMPLATE.html`. All links are root-relative, so the site will NOT work from a
subdirectory. Serve `404.html` for not-found routes.

The product's actual source code lives at `~/code/Maestro` (repo `chapterengine`).
Its `CLAUDE.md` / `README.md` / `docs/STATUS.md` are the source of truth for behavior —
verify claims there before documenting features, and check the do-not-market list in
Claude's project memory before announcing anything new.

## Sister sites (one studio, three domains)

- **implement.studio** (`~/Herd/implementstudio`) — the studio's own site; announces
  Maestro Studio, Maestro Vision, ChapterScript and ChapterPlayer.
- **maestrostud.io** (this repo) — the Maestro product experience + handbook.
- **chapterscript.com** (`~/Herd/chapterscript`) — the open source ChapterScript
  format + ChapterPlayer runtime.

Cross-link with absolute `https://` URLs; the sites deploy as separate GitHub Pages repos.

## Branding rules (strict — the #1 thing to not get wrong)

The 2026-08-12 rebrand flipped the old naming: "Maestro" (once the internal codename)
is now the public brand, and the "Chapter*" names survive only in the open source
format/runtime.

| Public name (use on site) | What it is |
|---|---|
| **Maestro** (the suite) | The whole product family |
| **Maestro Studio** | The Mac authoring app (ChapterStudio target internally) |
| **Maestro Vision** | The Vision Pro app (ChapterVision target internally) |
| **ChapterScript** / `.chapterscript` bundle / `chapter.json` | The open format (public, open source) |
| **ChapterPlayer** | The open source player runtime (SharedVisions codename stays hidden) |
| the particle editor | Afterburn codename stays hidden |

- **Never on the site:** `Chapter Vision`, `Chapter Studio`, `Vision Studio`,
  `SharedVisions`, `Afterburn`, `MaestroKit`, `chapterengine`, target names,
  `_maestro._tcp` / `_chaptervision._tcp`. ("Chapter Vision"/"Chapter Studio" are the
  RETIRED public names — fix on sight.)
- Suite vs app: platform-level statements ("Maestro saves everything as ChapterScript")
  use bare **Maestro**; anything the headset app does is **Maestro Vision**; anything
  the Mac app does is **Maestro Studio**. Never bare "Studio" or "Vision" as app names.
- Wordmark renders in **title case**, never all-caps. Docs title pattern:
  "… — Maestro Handbook".
- Document model vocabulary (format v3, adopted sitewide 2026-08-12): a **chapter**
  contains **sequences** → **steps** → **actions**; sequences carry animation tracks,
  a presentation mode (Immersive/Mixed/Windowed), an optional backdrop; steps can
  have **gates** (Tap / Gaze at Entity / Approach Entity / Grab Entity + optional
  timeout and prompt). "Segment" is the RETIRED v2 word — fix on sight (exception:
  "segmented picker", the UI control). Orchestrator and Either gate types are STILL
  unannounced — they must not appear.
- **Positioning (2026-08-12): Maestro is NOT publicly released.** The site onboards
  founding users via mailto:hello@maestrostud.io (homepage `#early-access` section).
  No download links, no pricing, no public "get the app" CTAs. Docs may still describe
  installation/setup for users who have builds.
- Docs voice: second person, present tense, for creative authors not programmers. No
  Swift type names or source paths (live-sync + format pages may show HTTP/JSON).

## Theme (derived from the real app icon — don't invent new colors)

Palette sampled from the app icon (`assets/icon-256.png` / `icon-512.png`, an open
storybook with a glowing portal). The icon is the brand mark everywhere; no SVG logo.

Tokens in `:root` of `assets/css/site.css`: backgrounds `--bg #0d0c1d`, `--bg-2
#14122a`, `--bg-3 #1d1a38`; text `--ink #f0ecf9`, `--ink-dim #a49ec6`, `--ink-faint
#6b6591`; accents `--peri #9c8cf2`, `--gold #f3cf9a`, `--sky #6596dc`; gradients
`--grad-glow` (peri→gold, CTAs), `--grad-aurora` (sky→peri→gold, gradient text).
Fonts: Fraunces (display), Outfit (body), JetBrains Mono. Dark text on gradient
buttons is literal `#14102e`. Use the CSS variables for anything new.

## File map

```
index.html                Homepage: chapter-structured scroll experience (nav,
                          progress bar, chapter rail scroll-spy, hero w/ canvas,
                          film video slots, marquee, chapters 01–09, final CTA,
                          footer). Anchors: #studio #spatial #animation #sync
                          #particles #everything #format #learn #early-access.
                          FIVE VIDEO PLACEHOLDERS remain (see Homepage videos).
404.html                  Themed not-found page (reuses hero canvas)
assets/css/site.css       ALL design tokens + homepage styles
assets/js/site.js         Homepage interactions (scroll scrubbing, reveals,
                          rail scroll-spy, hero canvas)
assets/icon-256.png/512   App icon brand mark + favicon
assets/video/             Encoded web videos (film.* is the filled hero slot)
scripts/encode-web-video.sh   The video encode pipeline (see below)
docs/                     Handbook: flat pages + _TEMPLATE.html + assets/docs.js
                          (MANIFEST = single source of truth for nav/search)
```

## How docs pages work

Every docs page is a thin shell: `<head>` → `<body class="docs">` →
`<div id="docs-shell"></div>` → `<main class="doc-main"><article class="doc-article"
data-page="SLUG">…</article></main>` → `<script src="/docs/assets/docs.js">`.
Chrome (header, sidebar, search, TOC, prev/next) is injected by docs.js at runtime.

**To add a page:** copy `docs/_TEMPLATE.html` → `docs/<slug>.html`; set `<title>`
("… — Maestro Handbook"), meta description, `data-page="<slug>"` (MUST equal the
filename); add `{ id, file, title, desc, keywords }` to the right section of
`MANIFEST` in `docs/assets/docs.js`.

34 content pages + index. Notable renamed slugs (2026-08-12): `what-is-maestro`
(was what-is-chapter-vision), `maestro-vision-overview` (was vision-studio-overview).

## Homepage videos (the repeatable pipeline)

Six film slots. As of 2026-08-12 the **studio-timeline** slot temporarily shows the
product film (assets `film.*`, moved from the hero slot at the user's request) until
a dedicated timeline capture exists. Placeholders: **film** (hero), **spatial-editing**,
**graph-editor**, **live-sync**, **particles** (footage specs in each slot's HTML
comment). A new hero capture should use a fresh slug since `film.*` is occupied.

To fill a slot: drop the raw capture in `videos/` (gitignored, never commit masters),
run `scripts/encode-web-video.sh videos/<capture> <slug>` (optional third arg = poster
timestamp, default 1s), then replace that slot's `<div class="vp">…</div>` with the
printed `<video>` markup (AV1 source first, H.264 fallback, poster, autoplay muted
loop playsinline). Keep `data-scrub` on the `.film-frame`. Verify in a NORMAL browser
(maestrostudio.test) — the Claude-in-Chrome automated browser blocks all media
playback and only shows posters.

Encode settings are VMAF-tuned (2026-07-30): SVT-AV1 10-bit preset 3 CRF 43 (≈VMAF 95,
10-bit prevents banding on the dark indigo gradients) + x264 veryslow CRF 26 fallback,
audio stripped, max width 1920. If a clip looks soft, drop AV1 CRF to ~40 and measure
with the libvmaf ffmpeg filter rather than guessing.

## Verification (run after any sitewide change)

```bash
cd ~/Herd/maestrostudio

# manifest ↔ files agree, no orphans
node -e "const fs=require('fs');const js=fs.readFileSync('docs/assets/docs.js','utf8');
const m=[...js.matchAll(/file: \"([^\"]+)\"/g)].map(x=>x[1]);
const f=fs.readdirSync('docs').filter(x=>x.endsWith('.html')&&x!=='_TEMPLATE.html');
console.log('missing:',m.filter(x=>!f.includes(x)),'orphans:',f.filter(x=>!m.includes(x)))"

# data-page matches filename
for f in docs/*.html; do b=$(basename $f .html); [ "$b" = _TEMPLATE ] && continue; \
  grep -q "data-page=\"$b\"" $f || echo "MISMATCH: $f"; done

# no broken internal links
grep -rhoE 'href="/(docs/)?[^"#]*\.html"' index.html 404.html docs/*.html | \
  sed 's/href="//;s/"//' | sort -u | while read u; do [ -f ".$u" ] || echo "MISSING: $u"; done

# no retired names or internal codenames leaked (multiline-aware; &nbsp; too)
perl -0ne 'print "LEAK: $ARGV\n" if /Chapter(\s|&nbsp;|&#160;)+(Vision|Studio)|Vision\s+Studio|SharedVisions|Afterburn|MaestroKit|chapterengine|chaptervision|chapter\.vision/i' \
  index.html 404.html README.md docs/*.html docs/assets/*.js assets/js/*.js assets/css/*.css

# pages actually serve
curl -s -o /dev/null -w "%{http_code}\n" http://maestrostudio.test/
```

## Gotchas

- **Renames must be contextual.** App names wrap across source lines and hide in
  `&nbsp;` entities, so plain `sed`/grep miss them — use `perl -0` multiline patterns
  that also match `&nbsp;`, and re-grep afterwards. "spatial video", "spatial
  storytelling", "Spatial gizmos" are generic, not app references. Beware suite-vs-app:
  a rename that flattens "Maestro Vision" and "Maestro" into one string loses meaning.
- **docs.js MANIFEST is the single source of truth** for docs nav/search; a page not
  in it is invisible. docs.js also injects the header wordmark (a string in JS, not
  HTML — include it in rename sweeps).
- **Video encoding:** never re-encode by hand or commit raw captures.
- `prefers-reduced-motion` is honored everywhere; keep that true for anything new.
- Em dash policy: body prose keeps at most ~1–3 per page. The em dash in `<title>`
  tags is the title pattern and stays. Don't reintroduce dash-heavy copy.
- Do-not-market list (Claude project memory, 2026-08-07): group rigs, clip
  speed/retiming-as-rate, multi-backdrop device playback, welcome-window
  Templates/Demos/Tutorials are NOT shippable — keep them off the site.
