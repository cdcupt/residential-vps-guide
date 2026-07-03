# Provider dataset — network builder

Source of truth for the builder's roster + prices. **Benchmarked** = a box we run and
measured (real perf/latency/IO/route). **Reference** = official price/spec only, not
benchmarked by us. Prices are curated snapshots ("as of 2026-07-03"); mark stock.

Currencies: USD unless noted. VMISS is **CAD** (≈0.73 USD). AaITR is CNY (¥, ≈0.147 USD).

---

## JUMP HOSTS (datacenter · CN2 GIA)

### BandwagonHost  — benchmarked (US-LA) + reference (other regions)
All plans are CN2 GIA (US-LA GIA-E; SG/HK/JP show "China Telecom CN2 GIA" in DC highlights).
Our fleet's bwh-prod (US-LA 4C/4GB, **perf 83**) and bwh-vpn (US-LA 3C/2GB, **perf 75**)
are the benchmarked rows; latency 129 ms / 0% loss measured; disk ~57k IOPS.

| Region (DC) | Plan | Disk | RAM | CPU | Transfer | Link | Price/mo | Note |
|---|---|---|---|---|---|---|---|---|
| US-LA · GIA-E | — | 20GB | 1GB | 2 | 1TB | 2.5G | **$16.66** ($49.99/3mo) | ref |
| US-LA · GIA-E | bwh-vpn | 40GB | 2GB | 3 | 2TB | 2.5G | **$30.00** ($89.99/3mo) | **benchmarked perf 75** |
| US-LA · GIA-E | bwh-prod | 80GB | 4GB | 4 | 3TB | 2.5G | **$56.99** | **benchmarked perf 83** |
| US-LA · GIA-E | — | 160GB | 8GB | 6 | 5TB | 5G | **$86.99** | ref |
| Singapore · SG_8 (Equinix SG1) | — | 40GB | 2GB | 2 | 500GB | 1.5G | **$49.99** | ref (CN2 GIA · CMIN2 · Unicom Premium) |
| Singapore | — | 80GB | 4GB | 4 | 1TB | 1.5G | **$86.99** | ref |
| Singapore | — | 160GB | 8GB | 6 | 2TB | 2.5G | **$165.99** | ref |
| Singapore | — | 320GB | 16GB | 8 | 4TB | 2.5G | **$329.99** | ref |
| Singapore | — | 640GB | 32GB | 10 | 6TB | 5G | **$549.99** | ref |
| Hong Kong · HKHK_8 (Equinix HK2) | — | 40GB | 2GB | 2 | 500GB | 1G | **$89.99** | ref (CN2 GIA peering) |
| Hong Kong | — | 80GB | 4GB | 4 | 1TB | 1G | **$155.99** | ref |
| Hong Kong | — | 160GB | 8GB | 6 | 2TB | 1G | **$299.99** | ref |
| Hong Kong | — | 320GB | 16GB | 8 | 4TB | 1G | **$589.99** | ref |
| Hong Kong | — | 640GB | 32GB | 10 | 6TB | 1G | **$989.99** | ref |
| Japan-Osaka · JPOS_6 (Equinix OS1) | — | 40GB | 2GB | 2 | 500GB | 1.5G | **$49.99** | ref (CN2 GIA peering) |
| Japan-Osaka | — | 80GB | 4GB | 4 | 1TB | 1.5G | **$86.99** | ref |
| Japan-Osaka | — | 160GB | 8GB | 6 | 2TB | 1.5G | **$165.99** | ref |
| Japan-Osaka | — | 320GB | 16GB | 8 | 4TB | 1.5G | **$329.99** | ref |
| Japan-Osaka | — | 640GB | 32GB | 10 | 6TB | 1.5G | **$549.99** | ref |

### DMIT — reference (CN2 GIA "Pro" line, perpetually out of stock)
Source: local dmit-watch panel (`localhost:7331/api/state`) for LAX; PM screenshots for HK/JP.

| Region | Plan | Specs | Transfer | Price/mo | Stock |
|---|---|---|---|---|---|
| US-LA | LAX.AS3.Pro.TINY | 1C/512M-ish | — | **$10.90** | out |
| US-LA | LAX.AS3.Pro.STARTER | — | — | **$34.90** | out |
| US-LA | LAX.AS3.Pro (range) | — | — | $10.90–$199.90 | out |
| Hong Kong | HKG.AS3.Pro.TINY | 1 vCore · 1GB · 20GB SSD · Premium | 500GB @ 1Gbps | **$39.90** | out |
| Japan-Tokyo | TYO.AS3.Pro.TINY | 1 vCore · 1GB · 20GB SSD · Premium | 500GB @ 1Gbps | **$21.90** | out |

### VMISS — reference (US-LA CN2 GIA, prices in CAD, out of stock)
`app.vmiss.com/store/us-los-angeles-cn2` — many other regions exist (HK-BGP, JP-Osaka/Tokyo, KR, US-LA TRI/9929/CMIN2); PM referenced US-LA CN2 GIA.

| Plan | Specs | Port | Bandwidth | Price/mo | Stock |
|---|---|---|---|---|---|
| US.LA.CN2.Basic | 1C · 1GB · 10GB SSD | 200Mbps | 300GB | **$6 CAD** (~$4.40) | out |
| US.LA.CN2.Core | 1C · 1GB · 15GB SSD | 200Mbps | 600GB | **$12 CAD** (~$8.80) | out |
| US.LA.CN2.Pro | 1C · 2GB · 20GB SSD | 500Mbps | 1000GB | **$20 CAD** (~$14.60) | out |

### VirVM (as jump host) — benchmarked (163 line; 专线GIA is a paid upgrade)
Our box: 4C/8GB, **perf 38**, 163 route (NOT CN2 GIA unless the 专线GIA upgrade is bought). Traffic-metered — see residential.

---

## RESIDENTIAL EXITS

| Provider | Bench? | Specs | Network | Perf | Price | Stock |
|---|---|---|---|---|---|---|
| **VirVM** | ✅ | 4C/8GB | 163 (专线GIA upgrade avail) | 38 | **traffic-metered** ↓ | in |
| **SolaDrive** | ✅ | 4C/4GB · 48GB | 163 · clean IP · 0% loss | 88 | **$40.00/mo** | in |
| **AaITR** | ✅ | 2C/2GB · 25GB | 163 · AT&T/Frontier · lossy (40–70%) | 18 | **~$21.88/mo** (¥149) | in |
| **QQ.pw** | ref | from 2C/3GB | Hawaii · residential | — | **$35–55/mo** | out |

### VirVM traffic-metered pricing (专线GIA residential) — **PM-PENDING prices**
Packages **100 / 300 / 500 / 1000 GB**; billing cycle 1mo · 3mo −5% · 6mo −10% · 1yr −20% · 2yr −25%.
Base + package $ still owed by the PM (structure final).

---

## Notes for the build
- **Multi-location/plan scale:** BandwagonHost/DMIT/VMISS each span regions × plans. Proposed
  UX (reuses the inline-configurator pattern): palette = one **provider** card; a placed jump
  node grows a **region + plan** selector (like VirVM's traffic selector), price + specs update live.
- **Benchmarked rows** carry real perf/latency/IO/route; every other row is **priced-only, "not benchmarked"** badged.
- **Stock:** DMIT + VMISS + QQ.pw currently out of stock → show an "out of stock" badge but still price them.
- Convert CAD/CNY to USD for the total (show native + ≈USD), like the fleet dashboard.
