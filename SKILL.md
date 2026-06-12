---
name: font-guide
description: >
  Generate a standalone typography guide HTML page from a website URL or a list of font
  names. Invoke with /font-guide <url | font names> — the skill identifies the type
  system (families, weights, italics, roles) and produces a single self-contained HTML
  file in the Reblis font-guide layout: gradient header with "Aa" font chips, sticky
  anchor nav, face specimen cards, weight-and-style ramps, type scale, every headline ×
  body pairing combination, and glyph character sets. Trigger when the user says "font
  guide", "type guide", "typography page", "make a font guide for <site>", or invokes
  /font-guide. Authored by Reblis.com.
---

# Font Guide Generator — by Reblis

Generate a **standalone typography guide HTML file** from a URL or a list of fonts.

Usage: `/font-guide <url>` (scrape the site's fonts) or `/font-guide Inter, Sofia Sans
Condensed` (named Google Fonts, 1–6 families). The user may also assign roles
(display / body / mono); honor whatever structure they give.

The canonical example is `template-example.html` in this skill directory (the Dandré
type system: Sofia Sans Condensed ×6 styles + Inter ×3). **Read it before generating** —
it is the exact layout, CSS architecture, and component set to reproduce.

## Hard Rules (non-negotiable)

1. **Standalone file.** One HTML file; the only external references are Google Fonts
   stylesheets (they ARE the subject matter here, so they're required, not optional).
2. **Sticky anchor menu** (`position: sticky; top: 0`, frosted blur) linking every
   numbered section, same as the style-guide and palette-guide skills.
3. **Header** reuses the family header-bar: full-bleed gradient in the brand's colors
   (scrape them when given a URL; pick a tasteful dark neutral pair when given only font
   names), thin accent rule (`::after`), a row of **"Aa" font chips** — one per loaded
   style group — kicker ("Font Guide · <year> Edition"), brand mark in the title slot
   (sanitized inline SVG if the brand has one; otherwise a **wordmark set in the
   project's own H1/display font** — the heaviest loaded weight, white, with the site's
   H1 letter-spacing. Never substitute an off-brand face — no Pacifico or other script
   fallback), lede (weight 400, max-width 770px), uppercase meta line with family/style
   counts.
4. **Only loaded styles.** Show exactly the weights/italics the Google Fonts request
   ships — never demonstrate a weight that would faux-render. Include a callout with the
   canonical `family=...` request string.
5. **No header glow.** Never add a `::before` radial-gradient glow blob (e.g.
   `radial-gradient(circle, rgba(...), transparent 68%)`) to the header band — on any
   guide, for any brand. The header is a flat color or linear gradient only.
6. **Output location:** default `font-guide.html` in the relevant project folder. Never
   stream the HTML into chat.

## Scraping fonts from a URL

- Google Fonts `<link>` href carries the truth: families, weights, and the `ital` axis.
  Parse it rather than guessing from rendered CSS.
- Map roles from usage: which family styles headings vs body vs code (`font-family`
  declarations on h1/body/pre or CSS custom properties like `--display` / `--sans`).
- Grab brand colors (CSS custom properties) to theme the guide authentically.
- Sample copy: pull real phrases from the site (menu items, taglines) so specimens feel
  native, in the site's language.

## Sections

`01 · Faces` — one card per family: giant "Aa" specimen (~150px, heaviest weight), name,
classification (e.g. Condensed Display Sans / Neutral Grotesque / Humanist Serif /
Monospace), role, and a monospace spec line (weights · style count · roman/italic).

`02 · Weights & Styles` — one ramp card per family (analog of palette shade ramps): a row
per loaded style — label (Regular / Bold Italic…), monospace meta (weight number +
italic flag), and a real sample sentence rendered in that exact style. Display families
sample large (~30px); body families sample at text size (~17px).

`03 · Type Scale` — scale-bar rows (the style-guide pattern): each size names which
family owns it. Call out the display/body crossover point explicitly.

`04 · Pairings` — **every headline × body combination** (n² ordered pairs, including
same-family pairs): card with a headline in font A over body copy in font B, uppercase
label *below* the card in slate ("Sofia × Inter"), and the canonical pairing marked in
the label (accent color) — never badges overlapping the sample text.

`05 · Glyphs` — character-set block per family: A–Z, a–z, 0–9, plus the diacritics the
site's language needs (Spanish sites get Á É Í Ó Ú Ñ ¿ ¡). Note coverage in the blurb.

`footer` — brand + guide title left; monospace stamp right ("2 families · 9 styles · 4
pairings").

## Generate programmatically

Build the HTML with a small Python script (style loops, pairing products, counts) and
write the file in one shot — don't hand-write ramp rows and pairing cards.

## Verify before claiming done

Screenshot headlessly and **look at the images** — fonts need a `--virtual-time-budget`
so Google Fonts actually load before capture:

```bash
timeout 90 chromium --headless --disable-gpu --no-sandbox \
  --screenshot=check.png --window-size=1400,4600 --hide-scrollbars \
  --virtual-time-budget=8000 "file:///path/to/font-guide.html"
convert check.png -crop 1400x1550+0+<offset> slice.png
```

Snap-packaged chromium gotcha: private /tmp, no home-dir reads, and it can only WRITE
inside its own area — copy the HTML into `~/snap/chromium/common/`, render from there,
and give `--screenshot` an absolute path in that same directory. Check that no specimen
renders in a fallback font (a sample that looks like system sans = the webfont didn't
load). Clean up temp copies afterwards.
