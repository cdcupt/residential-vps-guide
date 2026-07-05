# Residential VPS Guide

A measured, **data-driven** field guide to choosing a VPS and building residential-exit VPN networks — grounded in real, uniform benchmarks rather than spec sheets and affiliate links.

**Live:** <https://residential-vps.daichenlab.com>

## What's inside

| Page | Topic |
|------|-------|
| `index.html` | Landing — the thesis, the architecture at a glance |
| `choose.html` | **Choose a VPS** — residential vs datacenter, decoding specs, IP purity, a ten-minute benchmark method, the fleet value table, red flags, a decision checklist |
| `build.html` | **Build a network** — client → relay → residential exit architecture, Reality tunnel, forward-proxy exit, security, operations, hard-won lessons |
| `tool.html` | **Network Builder** — assemble a jump→exit(s) network from the full provider catalog for a live price + a six-metric estimate; or score any single box 0–100 |
| `setup.html` | **Skill** — the `deploy-vps-network` agent skill (copy or download): every shell command for benchmarking, deploying, and operating an exit network |
| `deployment.html` | **Deployment** — the fleet, real measured numbers, and the end-to-end deploy walkthrough |

## Network Builder

`tool.html` assembles a residential-exit network — one jump host fanning out to one or more residential exits — and reads back, per box and for the whole route:

- **Price** — official list price per plan; out-of-stock boxes are still priced (with a badge) and every box carries a **Buy on official site** link.
- **Six estimate metrics** — price, latency range, disk IOPS, min-hop bandwidth, route / outbound-ISP, and **⑥ IP purity** — a green/amber/red traffic-light sourced from a third-party residential review.
- **Recommendation stars** and **benchmarked vs. reference** badges — the boxes we run are measured; the rest are shown at their official spec for comparison.

Everything is an estimate, runs entirely in your browser, and is labelled as such. The provider dataset lives in `docs/workflow/network-builder/PROVIDERS.md`.

## Design

A single self-contained static site: shared `style.css`, no build step, no framework, no trackers. Editorial-technical design (Fraunces + Inter). Everything after the fonts is local.

## Principles

- **Independent & vendor-neutral** — no affiliation with any provider, no affiliate links.
- **Methods, not live infrastructure** — provider names and measured numbers are public; IPs, hostnames, ports, and network topology stay withheld. Nothing here exposes anyone's live setup.
- **Measured where we can, sourced where we can't** — boxes we run are benchmarked with reproducible commands; everything else is official spec or attributed third-party review.

## License

[MIT](LICENSE) — use it, fork it, adapt it.
