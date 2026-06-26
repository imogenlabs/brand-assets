# Contributing

This is a **public** repository and the canonical store for all Imogen Labs product
imagery. It holds **images and the manifest only**.

## Ground rules

- **Never commit anything sensitive.** No `.env` files, credentials, API tokens,
  private keys, source code, customer data, or internal docs. This repo is public —
  assume everything you push is visible to the world, forever, in git history.
- **Images and metadata only.** Allowed file types: `.webp`, `.svg`, `.png` (the last
  for `brand/` assets only), plus `.json`, `.md`, `.yml`, and `.gitkeep` for repo
  structure. CI rejects anything else.
- **Capture from demo/dev environments** with demo accounts only — never production
  data or real customer information.
- **WebP for screenshots, dark + light for everything.** See `README.md`.
- **PR review is required.** Every change lands via pull request so a human can
  confirm no sensitive content was introduced and the naming convention holds. CI
  validates `manifest.json` and the committed file types on every PR.

## Workflow

1. Add files at the correct path and update `manifest.json`.
2. Open a PR. Wait for green CI.
3. A maintainer reviews for sensitive content and naming, then merges.
