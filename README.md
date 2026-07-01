# Residential VPS Guide

A measured, **data-driven** field guide to choosing a VPS and building residential-exit VPN networks — grounded in real, uniform benchmarks rather than spec sheets and affiliate links.

**Live:** <https://residential-vps.daichenlab.com>

## What's inside

| Page | Topic |
|------|-------|
| `index.html` | Landing — the thesis, the architecture at a glance |
| `choose.html` | **How to choose a VPS** — residential vs datacenter, decoding specs, IP quality, a ten-minute benchmark method, red flags, a decision checklist |
| `build.html` | **How to build a VPS network** — client → relay → residential exit architecture, Reality tunnel, forward-proxy exit, security, operations, hard-won lessons |
| `tool.html` | **Score tool** — enter benchmark numbers, get one comparable 0–100 score (runs entirely in your browser) |

## Design

A single self-contained static site: shared `style.css`, no build step, no framework, no trackers. Editorial-technical design (Fraunces + Inter). Everything after the fonts is local.

## Principles

- **Independent & vendor-neutral** — no affiliation with any provider, no affiliate links.
- **Methods, not infrastructure** — every example uses generic placeholders and anonymized findings. Nothing here exposes anyone's live setup.
- **Measured** — every claim is backed by benchmarks anyone can reproduce with the commands in the guide.

## License

[MIT](LICENSE) — use it, fork it, adapt it.
