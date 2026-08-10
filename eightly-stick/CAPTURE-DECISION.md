# Eight.ly Stick — capture decision (SITE-175 spike)

**Decision: DEFERRED.** No captures ship for Eight.ly Stick until its host is
stable and the Stick UIs are reachable again. Until then the Stick product page
carries no imagery — plain text over placeholder or invented art.

## What was attempted (2026-08-10)

The Stick exposes browser UIs on ports 3000 and 3333 on amd-beast, a Windows
machine reached over Tailscale at 100.70.213.111.

- ICMP over Tailscale: **reachable** (0% loss).
- `http://100.70.213.111:3000/` — **timed out** (no listener).
- `http://100.70.213.111:3333/` — **timed out** (no listener).

## Why it failed

The host answers but the Stick services are not running. amd-beast is currently
under active crash investigation; the Stick UIs are down with it. Remotely
starting services on a machine mid-investigation is outside the bounds of a
capture spike, so no further attempts were made. The time-box was honored.

## Chosen treatment

- **Deferred**, not diagram and not placeholder: the Stick has a real UI, so an
  authored diagram would misrepresent a product whose genuine screens are only
  temporarily unavailable, and placeholder art is off-brand by policy.
- The manifest records the skip (see the `skipped` array in `manifest.json`) so
  site tooling can tell "no asset yet, on purpose" from "asset missing".

## How to revisit

When amd-beast is stable and ports 3000/3333 answer:

1. Add `scripts/capture/configs/eightly-stick.json` in imogenlabs-site
   (copy the agent-templates config; source URLs `http://100.70.213.111:3000`
   and `:3333`; confirm the theme mechanism on the live UI).
2. Run the capture engine with `--config eightly-stick`, output here under
   `eightly-stick/desktop/{theme}/`.
3. Remove the `skipped` entry from `manifest.json` and delete this file's
   "DEFERRED" status (keep the history).
