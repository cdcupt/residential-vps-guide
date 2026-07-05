# Provider dataset — network builder (FULL official catalogs)

Policy (PM, 2026-07-04): **list ALL official plan options per provider**, not just the boxes we run.
**Benchmarked** = a box we run + measured (real perf/latency/IO/route) — only a few rows.
Everything else = official price/spec, **not benchmarked** (priced-only). Mark stock.

Currencies: USD unless noted. VMISS = **CAD** (≈0.73 USD). AaITR = **CNY ¥** (≈0.147 USD).
Cycle discounts where a provider offers them: 3mo −5% · 6mo −10% · 1yr −20% · 2yr −25%.

---

## BandwagonHost — CN2 GIA · 4 regions (all reference except the 2 US-LA rows we run)
Order page: bandwagonhost.com. All plans RAID-10 SSD, CN2 GIA.

### US-LA · USCA_9 Coresite LA2 (CN2 GIA · CMIN2 · Unicom Premium)
| Disk | RAM | CPU | Transfer | Link | Price/mo | Note |
|---|---|---|---|---|---|---|
| 20GB | 1GB | 2 | 1TB | 2.5G | $16.66 ($49.99/3mo) | ref |
| 40GB | 2GB | 3 | 2TB | 2.5G | **$30.00** ($89.99/3mo) | **benchmarked bwh-vpn · perf 75** |
| 80GB | 4GB | 4 | 3TB | 2.5G | **$56.99** | **benchmarked bwh-prod · perf 83** |
| 160GB | 8GB | 6 | 5TB | 5G | $86.99 | ref |
| 320GB | 16GB | 8 | 8TB | 5G | $159.99 | ref |
| 640GB | 32GB | 10 | 10TB | 10G | $289.99 | ref |
| 1TB | 64GB | 12 | 12TB | 10G | $549.99 | ref |
| 1TB | 64GB | 12 | 15TB | 10G | $679.00 | ref |
| 1TB | 64GB | 12 | 20TB | 10G | $899.00 | ref |

### Hong Kong · HKHK_8 Equinix HK2 (CN2 GIA peering)
| 40GB/2GB/2C/500GB/1G $89.99 · 80GB/4GB/4C/1TB/1G $155.99 · 160GB/8GB/6C/2TB/1G $299.99 · 320GB/16GB/8C/4TB/1G $589.99 · 640GB/32GB/10C/6TB/1G $989.99 · 1TB/64GB/12C/8TB/1G $1,889.99 | (all ref, all /mo) |
|---|---|

### Japan-Osaka · JPOS_6 Equinix OS1 (CN2 GIA peering)
| 40GB/2GB/2C/500GB/1.5G $49.99 · 80GB/4GB/4C/1TB/1.5G $86.99 · 160GB/8GB/6C/2TB/1.5G $165.99 · 320GB/16GB/8C/4TB/1.5G $329.99 · 640GB/32GB/10C/6TB/1.5G $549.99 · 1TB/64GB/12C/8TB/1.5G $1,059.99 | (all ref, /mo) |
|---|---|

### Singapore · SG_8 Equinix SG1 (CN2 GIA · CMIN2 · Unicom Premium)
| 40GB/2GB/2C/500GB/1.5G $49.99 · 80GB/4GB/4C/1TB/1.5G $86.99 · 160GB/8GB/6C/2TB/2.5G $165.99 · 320GB/16GB/8C/4TB/2.5G $329.99 · 640GB/32GB/10C/6TB/5G $549.99 · 1TB/64GB/12C/8TB/5G $1,059.99 | (all ref, /mo) |
|---|---|

---

## DMIT — CN2 GIA "Pro" line. **Stock is PER-PLAN** (dmit-watch panel, live 2026-07-04) + HKG.AN5 from DMIT site.
In-stock plans ARE summable in the builder; out-of-stock = reference/excluded. (Corrects the earlier "all out of stock".)

### US-LA — all OUT of stock
- **AS3**: TINY $10.90 · POCKET $16.90 · STARTER $34.90 · MINI $62.90 · MICRO $87.90 · MEDIUM $199.90
- **AN4**: MINI $72.90 · MICRO $102.90 · MEDIUM $239.90 · LARGE $459.90 · GIANT $929.90
- **AN5**: MINI $79.90 · MICRO $110.90 · MEDIUM $289.90 · LARGE $499.90 · GIANT $1009.90

### Hong Kong
- **AS3 — IN STOCK**: TINY $39.90 · STARTER $79.90 · MINI $126.90 · MICRO $179.90 · MEDIUM $239.90
- **AN5** (from DMIT site — not panel-tracked, treat as available): MINI $149.90 · MICRO $199.90 · MEDIUM $279.90 · LARGE $359.90 · GIANT $759.90
  - specs: MINI 4C/4GB/80GB/1500GB · MICRO 4C/4GB/160GB/2000GB · MEDIUM 6C/8GB/160GB/2500GB · LARGE 8C/16GB/320GB/3000GB · GIANT 8C/24GB/640GB/6000GB (Premium routing, 1Gbps)

### Japan-Tokyo — AS3
- **IN STOCK**: TINY $21.90 · STARTER $45.90 · MINI $89.90 · MICRO $189.90 · MEDIUM $320.90
- OUT: LARGE $429.90 · GIANT $829.90

TINY tiers ≈ 1 vCore / 1GB / 20GB SSD / Premium routing / 500GB @ 1Gbps.

---

## VMISS — US-LA CN2 GIA only (CAD, out of stock). app.vmiss.com/store/us-los-angeles-cn2
| Plan | Specs | Port | Bandwidth | Price/mo |
|---|---|---|---|---|
| US.LA.CN2.Basic | 1C·1GB·10GB | 200Mbps | 300GB | $6 CAD (~$4.40) |
| US.LA.CN2.Core | 1C·1GB·15GB | 200Mbps | 600GB | $12 CAD (~$8.80) |
| US.LA.CN2.Pro | 1C·2GB·20GB | 500Mbps | 1000GB | $20 CAD (~$14.60) |
| US.LA.CN2.Elite | 2C·4GB·40GB | 500Mbps | 1600GB | $38 CAD (~$27.70) |
| US.LA.CN2.Ultra | 4C·8GB·80GB | 1000Mbps | 2800GB | $75 CAD (~$54.75) |
All reference · out of stock. (PM: focus ONLY on US-LA CN2 GIA; ignore VMISS's other regions.)

---

## QQ.pw — Hawaii · residential · reference. qq.pw/store/residential-vds-with-dedicated-ip
### Hawaii Dedicated VDS (dedicated residential IP, AMD 7940HS) — all 0 available (out of stock)
| Plan | Specs | Data | Price |
|---|---|---|---|
| Dedicate IP VDS Intern | 2vCore·3GB·20GB NVMe | 1TB@400Mbps + Unlim 35Mbps | $35.00/mo |
| Dedicate IP VDS Reliable | 3vCore·4GB·30GB | 2TB@400Mbps + Unlim 35Mbps | $35.00/mo |
| Dedicate IP VDS Hardcore | 4vCore·4GB·40GB | 4TB@400Mbps + Unlim 35Mbps | $45.00/mo |
| Dedicate IP VDS Elite | 5vCore·8GB·50GB | 5TB@400Mbps + Unlim 35Mbps | $165.00/quarterly |

### Hawaii Residential NAT VPS ("ideal for AI tools") — in stock
| Plan | Specs | Data | Net | Price |
|---|---|---|---|---|
| NAT Tiny | 1vCore·238MB·4GB | 1GB/mo | 10Mbps sym | $12/yr |
| NAT Lite | 1vCore·256MB·4GB | 6GB/mo | 50Mbps | $2/mo |
| NAT Basic | 1vCore·512MB·4GB | 20GB/mo | 100Mbps | $4/mo |
| NAT Basic2 | 1vCore·512MB·4GB | 50GB/mo | 150Mbps | $6/mo |
| NAT Basic3 | 1vCore·512MB·4GB | 100GB/mo | 200Mbps | $8/mo |
| NAT Standard | 1vCore·512MB·4GB | 200GB/mo | 250Mbps | $10/mo |
| NAT Booster | 1vCore·1GB·10GB | 512GB/mo | 300Mbps | $16/mo |
| NAT Plus | 2vCore·1.5GB·15GB | 1TB/mo | 350Mbps | $22/mo |
| NAT Premium | 2vCore·2GB·20GB | 2TB/mo | 400Mbps | from $28/mo |
| NAT Extreme | 8vCore·8GB·80GB | 40TB/mo | 1Gbps · dedicated NAT range | $690/mo |

---

## AaITR — CA/JP residential · CNY ¥149/mo ≈ **$21.88**. Buy: aaitr.com/store/srv
### Static residential VPS (独享静态 · 2vCPU·2GB·25GB SSD·100Mbps·2000GB) — SOLD OUT
| US AT&T static ¥149 (真实民宅, CA multi-city) · US Frontier static ¥149 | **benchmarked: our AaITR box = this line, perf 18** |
|---|---|

### Residential NAT VPS (共享动态 · 1vCPU·512MB·8GB SSD·1000GB · port-forward ×10) — in stock
| US Frontier NAT ¥149 (100Mbps, CA, daily IP rotate) · JP SoftBank NAT ¥149 (50Mbps, Tokyo) |
|---|

---

## VirVM (vircs) — TWO product lines (vircs.com). Our box = residential 100M tier (perf 38).
### VirVM Residential (节点K · trusted-IP/port-forward · unlimited traffic · flat bandwidth tier)
| Bandwidth | Price/mo |
|---|---|
| 50 Mbps | **$35.99** |
| 100 Mbps | **$65.99** ← benchmarked (our box) |
| 200 Mbps | **$115.99** |
| 300 Mbps | **$160.99** |
Cycle discounts apply (3mo −5% … 2yr −25%). Specs: 4C·8GB·50GB NVMe, 1 IPv4, AT&T SF residential.

### VirVM GIA (专线隧道B · relay-ready no-whitelist · 300M dedicated · TRAFFIC-METERED)
| Traffic package | Price/mo |
|---|---|
| 100 GB | **$59.98** |
| 300 GB | **$99.98** |
| 500 GB | **$139.98** |
| 1000 GB | **$239.98** |
Cycle discounts apply. Specs: 300M dedicated, 1 IPv4, VLESS+Reality, Asia-Pacific↔NA corridor.

---

## SolaDrive — US-LA residential (163 · clean IP · 1Gbps port). "Residential IP" line. Our box = SD-4 (perf 88). Buy: soladrive.com/support/store/residential-ip
| Plan | Specs | Res IP | Transfer | Price/mo |
|---|---|---|---|---|
| SD-2 | 2C @3.0GHz+ · 2GB · 25GB Raid-10 NVMe | 1 | 2TB | $25.00 |
| SD-4 | 4C @3.0GHz+ · 4GB · 50GB NVMe | 2 | 4TB | **$40.00** ← benchmarked (our box · perf 88) |
| Residential IP - 10 VPS | 10× (2GB · 25GB NVMe each) | 10 | 15TB | $195.00 |
| Residential 250 VPS Node | 24C/48T @3.3GHz · 512GB DDR4 · 2×8TB SSD Raid-1 · SolusVM/ESXi · fully managed | 254 | 30TB | $795.00 |
All in stock · 1Gbps port. Only SD-4 is benchmarked (our fleet exit); the other three are priced-only / not benchmarked.

---

## Roster roles (which slot each fills in the builder)
- **Jump host (datacenter CN2 GIA):** BandwagonHost, DMIT, VMISS.
- **Residential exit:** VirVM (residential + GIA), SolaDrive, AaITR, QQ.pw.
- **Benchmarked (our fleet, real metrics):** BWH US-LA 3C/2GB (75) & 4C/4GB (83); VirVM residential 100M (38); SolaDrive SD-4 $40 (88); AaITR static ¥149 (18). Everything else = priced-only / not benchmarked.
- **Pricing models the engine must handle:** flat $/mo · flat + billing-cycle-discount · **bandwidth-tiered** (VirVM residential) · **traffic-metered** (VirVM GIA — bring the metered model back for this SKU) · currency conversion (CAD, CNY→USD).
- **Stock:** DMIT, VMISS, QQ.pw-dedicated, AaITR-static currently out → badge but still price; out-of-stock excluded from the buildable total (reference), only in-stock summed.

## Recommended (★ star on provider cards)
The builder marks the most-recommended providers with a **★ star** on both the palette card and the placed-node card. Basis: **the PM's own top pick + [meowvps.com](https://meowvps.com/blog/resrec/)'s residential roundup ratings** (1–5 scale) + our own use. We do **not** invent ratings for providers the roundup doesn't list.

| Provider | Badge | Basis |
|----------|-------|-------|
| **VirVM** | **★ Top pick** | PM's #1 (most mature US-West residential provider) + meowvps roundup **5/5** |
| **AaITR** | ★ Recommended | meowvps roundup **5/5** |
| **QQ.pw** | ★ Recommended | meowvps roundup **5/5** |
| **BandwagonHost** | ★ Recommended | our own go-to CN2 GIA jump host (not in the residential roundup) |
| SolaDrive | — (no star) | meowvps roundup **3/5** · IP-degrading over time |
| DMIT | — (no star) | out-of-stock reference (not rated in the roundup) |
| VMISS | — (no star) | out-of-stock reference (not rated in the roundup) |

`★ Top pick` renders as the standout (accent-filled badge); `★ Recommended` is accent-tinted. Each badge carries a `title` tooltip crediting the basis. QQ.pw's roundup score (5/5) is the **recommendation basis**; its ⑥ IP PURITY reads `excellent · ref · meowvps` — a separate axis, but both drawn from the same meowvps review.

## IP purity (meowvps residential review)
The builder's **⑥ IP PURITY** metric reflects the **egress IP** — which is always the **exit's** IP, never the jump's. It is a **qualitative rating sourced entirely from [meowvps.com](https://meowvps.com/blog/resrec/)'s residential review** — **not a scorer number**. The earlier reputation-scorer number (**Scamalytics · IPQS · AbuseIPDB**, "purity = 100 − fraud score") was **dropped for every machine**: too small a sample to publish a per-provider figure. Ranking, worst→best: **variable · degrading < good < excellent**.

| Provider | ⑥ IP purity | Source | Basis (meowvps review) |
|----------|-------------|--------|------------------------|
| **VirVM** (residential + GIA) | **excellent** | meowvps | "IP质量优秀" (minor caveat: a few old IP ranges weaker) |
| **AaITR** (static/NAT residential) | **excellent** | meowvps | "IP质量优秀" |
| **QQ.pw** (Hawaii residential) | **excellent** | meowvps | "ip质量优秀 / 最稳定" |
| **SolaDrive** (163 · clean IP) | **variable · degrading** | meowvps | 2nd-tier: recycles IP ranges, some IPs hit Google captcha |

Jump hosts (BandwagonHost, DMIT, VMISS) are datacenter boxes and never appear in ⑥ — the metric reflects the **exit**, not the jump. In the builder: **rated exit** → `<rating> · ref · meowvps` (e.g. `excellent · ref · meowvps`; SolaDrive → `variable · degrading · ref · meowvps`); **unrated exit** (no blog read) → `— · unrated`; **multiple exits** → the **worst (lowest-ranked)** rating, so any SolaDrive present surfaces the `variable · degrading` caveat; **zero exits** (single-host) → `datacenter IP · no residential exit` (the jump's own datacenter egress, not rated).
