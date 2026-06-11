# font-guide — a Claude Code skill by [Reblis.com](https://reblis.com)

Turn a website's type system into a polished, standalone **typography guide HTML page** with one command:

```
/font-guide https://example.com
```

or hand it fonts directly:

```
/font-guide Sofia Sans Condensed, Inter
```

Claude identifies the families, the exact weights and italics loaded, and their roles (display / body), then generates a single self-contained HTML file: gradient header with "Aa" font chips, sticky anchor navigation, face specimen cards, weight-and-style ramps, a type scale, **every** headline × body pairing combination, and glyph character sets.

Open [`template-example.html`](template-example.html) in a browser to see a finished guide — the Dandré type system: Sofia Sans Condensed in 6 styles (with true italics) paired with Inter.

## Install

```bash
git clone https://github.com/Reblis/font-guide-skill ~/.claude/skills/font-guide
```

Restart Claude Code (or start a new session) so the skill registers, then run:

```
/font-guide <url | font names>
```

## What you get

- **One file.** The only external references are the Google Fonts stylesheets — which are the subject matter.
- **Only real styles.** The guide shows exactly the weights and italics the Google Fonts request ships — nothing faux-bolded or browser-slanted — and includes the canonical `family=` request string in a callout.
- **Every pairing.** All n² ordered headline × body combinations, with the canonical pair marked.
- **Glyph coverage.** Character sets per family, including the diacritics the site's language needs.
- **Authentic theming.** Given a URL, the skill scrapes the brand's colors and real copy so specimens feel native.
- **Verified output.** The skill screenshots its own output headlessly (with a virtual-time budget so webfonts actually load) and inspects it before calling the job done.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- `curl` (for fetching the target site)
- Optional, for the self-verification step: `chromium` + ImageMagick

## Siblings

- [style-guide](https://github.com/Reblis/style-guide-skill) — full brand style guide from any website URL
- [palette-guide](https://github.com/Reblis/palette-guide-skill) — color palette guide from 1–12 hex codes

## License

MIT © Reblis.com
