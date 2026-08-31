# brand-assets

**The single source of truth for all Imogen Labs product imagery.**

Every screenshot, authored diagram, logo, mascot, and OG image used across the
Imogen Labs marketing sites lives here — captured once, stored once, consumed
everywhere by stable CDN URL. No more per-repo copies drifting out of sync.

This repo is public and images-only on purpose: a public, PR-reviewed,
images-only repo makes "nothing sensitive ever shipped" trivial to audit, and
git gives a version-controlled audit trail of every rename and re-capture.

Assets are served straight from GitHub over the [jsDelivr](https://www.jsdelivr.com/)
CDN — zero new infra. If git ever bloats from image churn, the identical folder
structure mirrors to R2 and only the URL host changes.

## Naming convention

```
{product}/{form-factor}/{theme}/{screen-slug}.webp     # screenshots
{product}/diagram/{theme}/{slug}.svg                   # authored visuals
brand/...                                              # logos, mascots, OG images
```

- **product** — top-level slug, may be nested: `eightly/os`, `eightly/backup-server`,
  `homelabarr/ce`, `neurohelper`, `agent-templates`.
- **form-factor** — `desktop` · `mobile` · `tablet` (and `diagram` for authored visuals).
  A product only gets the form-factor folders it actually has (see the capture matrix
  in the design spec — e.g. `eightly/agent` is desktop-only).
- **theme** — `dark` · `light`. **Every asset ships in both themes.**
- **screen-slug** — stable kebab-case (`dashboard`, `app-store`, `settings`, `network`).
  The same slug identifies the same screen across every theme and form factor.

Examples:

```
eightly/os/desktop/dark/dashboard.webp
eightly/os/desktop/light/dashboard.webp
eightly/container/mobile/dark/app-store.webp
eightly/mobile/tablet/light/home.webp
ipac/diagram/dark/architecture.svg
brand/mascots/pirate.svg
```

## Consuming an asset

Build the URL from the jsDelivr pattern, pinning a git ref (`@main`, a tag, or a
commit SHA) for cache stability:

```
https://cdn.jsdelivr.net/gh/imogenlabs/brand-assets@main/<path>
```

Concrete examples:

```
https://cdn.jsdelivr.net/gh/imogenlabs/brand-assets@main/eightly/os/desktop/dark/dashboard.webp
https://cdn.jsdelivr.net/gh/imogenlabs/brand-assets@main/homelabarr/ce/desktop/light/dashboard.webp
https://cdn.jsdelivr.net/gh/imogenlabs/brand-assets@main/brand/logos/imogen-labs.svg
```

Prefer reading `manifest.json` over guessing filenames — it is the machine-readable
index of every asset (path, dimensions, source, capture date). The `cdnBase` field
plus an entry's `path` give you the full URL.

> **No cache-buster query strings.** Versioning is done by git ref/tag in the URL,
> never `?v=123`.

## Adding a new asset

1. Drop the file at the correct path per the naming convention above.
2. Add a matching entry to `manifest.json` (`assets[]`) — see `manifest.schema.json`
   for the required shape (`product`, `screen`, `formFactor`, `theme`, `path` are
   mandatory).
3. Open a PR. CI validates the manifest parses and that only allowed file types were
   committed. A reviewer confirms nothing sensitive slipped in.

## Rules

- **WebP only for screenshots.** No PNG twins — one optimized format per screen.
  Diagrams are SVG (or WebP). PNG is allowed **only** under `brand/`.
- **Dark + light, always.** Every screen and diagram ships both themes.
- **No secrets, ever.** This repo is public. Images and the manifest only — never
  commit `.env`, credentials, tokens, source code, or anything sensitive. Captures
  must use demo/dev environments and demo accounts, never real customer data.

## Brand logos

`brand/logos/` holds the Imogen Labs company mark. The letterforms are Archivo
(SIL Open Font License) instanced at wght 680, sitting exactly on the 0–686 grid
with Archivo's optical overshoot removed, hand-kerned over nine pairs, and
**converted to outline paths** — no font ships with the artwork, and the glyphs
are the same shape on every platform.

| File | Use |
|---|---|
| `imogen-labs.svg` | Wordmark, `currentColor`. Inline it and it inherits the surrounding text colour. |
| `imogen-labs-dark.svg` | Wordmark in `#fafafa`, with clear space, for dark grounds. |
| `imogen-labs-light.svg` | Wordmark in `#09090b`, with clear space, for light grounds. |
| `imogen-labs-mark.svg` | The `IL` mark alone, `currentColor`. Cut from the same letterforms as the wordmark. |
| `imogen-labs-avatar-dark.svg` | `IL` on a `#09090b` tile. This is the LinkedIn/profile avatar. |
| `imogen-labs-avatar-light.svg` | `IL` on a `#fafafa` tile. |

The two `currentColor` masters take their colour from the page, so they are not
theme-keyed and carry no `manifest.json` entry. The four themed files do.

Regenerate from source with `assets.py` in the brand working folder; never
redraw the letterforms by hand.
