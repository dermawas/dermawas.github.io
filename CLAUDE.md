# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static source for **dermawan.net**, a single-page personal landing card for Suseno Dermawan, served via GitHub Pages with a custom domain. There is no build system, package manager, or test suite — the entire site is one self-contained `index.html` (inline `<style>` and `<script>`, no external JS/CSS files).

## Commands

- Preview locally: open `index.html` directly in a browser (no dev server or build step needed).
- Deploy: commit and push to `main` — GitHub Pages serves directly from the branch root.

## Files

- `index.html` — the entire site.
- `CNAME` — custom domain (`dermawan.net`); GitHub Pages reads this to serve the custom domain instead of the `github.io` URL.
- `.nojekyll` — disables Jekyll processing so GitHub Pages serves `index.html` as-is.

## Architecture

`index.html` is a single centered "card" (`.card`) on a dark background, styled to look like a business card:

- **Head**: GA4 `gtag.js` snippet (Measurement ID hardcoded inline) + Google Fonts preconnect/link (Playfair Display for the name, Inter for everything else).
- **Card body**: tagline → name → location → bio → a **Ventures** section (`.portfolio-list`) → footer.
- **Ventures section**: nested `<ul>` structure — top-level ventures (currently Forstra Digital) as `<li>`, with `.portfolio-sublist` used to nest a venture's products underneath it (e.g. Ledgerize under Forstra Digital) with an indent + left border. Each entry pairs a `.portfolio-link` with a `.portfolio-desc` line.
- **Footer**: a LinkedIn link plus a QR code that encodes the *same* LinkedIn URL. The URL is duplicated in two places that must be kept in sync manually: the `href` on `.linkedin-link`, and the `LINKEDIN_URL` JS variable near the bottom of the file. The QR code itself is generated client-side (via the `qrcodejs` CDN library) into a hidden temp element, then copied onto the visible `<canvas id="qr-canvas">` — this indirection exists because `qrcodejs` doesn't render straight to a canvas element you supply.

## Conventions

- Everything stays inline in `index.html` — don't split out separate CSS/JS files for a single-page card like this.
- Color palette: navy `#1e2d4a` (ink), gold `#c9a84c` (accent), cream `#f8f5ef` (card background), `#8a8a8a`/`#3d3d3d` for muted/body text. Reuse these rather than introducing new colors.
- Mobile breakpoint is `max-width: 480px` — keep the card scrollable on small viewports (avoid reintroducing `overflow: hidden` on `body`, which previously clipped content once the card grew taller than the viewport).
