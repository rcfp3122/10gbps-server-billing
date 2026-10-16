# 10gbps dedicated server: real throughput, true unmetered costs, and how to choose between flat, metered and 95th percentile plans

A 10gbps dedicated server moves payload at 1,250 MB/s when the port runs full — roughly 3.2 PB of transfer over a 30-day month, enough headroom for about 2,000 concurrent 1080p streams or 400 in 4K. The port is the straightforward part. What separates a good deal from an expensive surprise is the traffic billing model attached to it: two providers can sell the same ten gigabits and finish the month an order of magnitude apart on the invoice.

This guide covers the arithmetic that actually matters, the three billing models you'll be asked to pick between, and the full set of 10 Gbps plans we found published on provider plan pages — so you can benchmark any quote, including budget-oriented sellers like RackNerd, against real market numbers instead of marketing copy.

> **Quick picks**
>
> - Cheapest published full-unmetered 10 Gbps: Evoluso ST-2 at **€135/mo** (promo, normally €199) — dual Xeon E5-2620v4, 64 GB RAM, 500 GB SSD, Amsterdam.
> - Guaranteed 10 Gbps port with published specs and modern EPYC options: the ZetServers / X-ZoneServers lineup from **€659/mo** across 13 locations.
> - Prefer paying for measured usage: server.net's quota model starts around **$141/mo all-in**, plus a $30 setup fee.
> - Buying on budget first: 👉 [check RackNerd's current dedicated server deals](https://bit.ly/my-rn), then compare whatever you find against the verified tables below.

## What a 10 Gbps port actually delivers

At full utilization, a 10 Gbps link carries 1,250 MB/s of payload in each direction on a full-duplex port. Those are the raw port numbers; everything else follows from them.

| What the port can carry | Number |
| --- | --- |
| Payload at line rate | 1,250 MB/s (decimal units) |
| Transfer over a fully used 30-day month | ~3.24 PB |
| Simultaneous 1080p viewers at 5 Mbps each | ~2,000 |
| Simultaneous 4K viewers at 25 Mbps each | ~400 |
| Time to move a 1 TB dataset | 13 min 20 s |

Two caveats belong right next to that table. First, these are port-capacity figures, not application guarantees — disk speed, CPU load, protocol overhead and the remote endpoint each apply their own ceiling, a point Evoluso makes explicitly on its plan page. Second, the viewer math assumes 5 Mbps per 1080p stream and 25 Mbps per 4K stream; Unihost's planning guidance puts 4K anywhere from 15–25 Mbps, so the 400-viewer figure is the conservative end of a range, not a fixed promise.

## The billing model decides your final bill

Three models dominate this tier, and the differences are bigger than most comparison posts admit. The clearest worked example comes from server.net, which publishes its bandwidth add-ons as separate line items on top of the server price:

| Bandwidth plan | Added monthly cost | How it works |
| --- | --- | --- |
| 100 TB transfer quota | $33 | Burst up to 10 Gbps; $2 per additional TB |
| 3 Gbps 95th percentile | $360 | Burst to 10 Gbps, no cap; $0.18 per Mbps above commit |
| 5 Gbps 95th percentile | $600 | Same overage math |
| 5 Gbps unmetered port | $500 | Rate-limited port, flat price |
| 10 Gbps unmetered port | $1,000 | Full port speed, flat price |

Now the fine print that rarely gets spelled out:

**The advertised entry price is quota-capped.** server.net headlines "10Gbps dedicated servers from $141/mo" — that figure is the $108 single-CPU EPYC 4545P box plus the 100 TB plan, plus a $30 setup fee. Order the same hardware with a true flat-unmetered 10 Gbps port and the bandwidth line alone adds $1,000, putting the realistic total around $1,108/mo. Both are legitimate products, but they serve completely different workloads, and the gap between them is the entire story.

**A quota is a countdown clock if you can actually saturate the pipe.** At full line rate a 10 Gbps server pushes about 4.5 TB per hour, which empties a 100 TB quota in under 23 hours. If your workload can genuinely do that, the $2/TB overage is where the real cost lands.

**95th percentile rewards spikes and punishes steadiness.** server.net measures P95 in 5-minute intervals over the billing month on the higher of inbound or outbound traffic, discarding the top 5% of samples — so short bursts are effectively free. Sustained heavy egress is the opposite case: if your 95th percentile lands near 8 Gbps on a 3 Gbps commit, the bill works out to roughly $360 plus 5,000 excess Mbps × $0.18 ≈ $1,260 — more than the $1,000 flat-unmetered port. Spiky transfers favor P95; steady streaming favors flat unmetered or quota.

## The full plan comparison: every published 10 Gbps offer

The tables below are taken from each provider's published plan pages. Promo prices move, so treat them as benchmarks and re-check the configurator before ordering. Where two providers publish an identical lineup, they share a row.

| Provider | Cheapest 10 Gbps route | Hardware at that price | Traffic model | Notes |
| --- | --- | --- | --- | --- |
| Evoluso | €135/mo promo (reg. €199) | Dual Xeon E5-2620v4, 16c/32t, 64 GB, 500 GB SSD | Unmetered, full-duplex | Amsterdam; unmanaged by default; EPYC/NVMe options up to €1,199 |
| server.net | $141/mo + $30 setup | AMD EPYC 4545P, 48 GB DDR5, 960 GB NVMe | 100 TB quota, burst to 10 Gbps | Flat-unmetered 10 Gbps adds $1,000/mo; P95 plans $360–600 |
| Unihost | from €1,099.90/mo ex. VAT | 2× Xeon E5-2620v3, 64 GB, 4× 8 TB HDD (Germany) | Unlimited | 5 Gbps unmetered from €469.90; free basic management; US/Germany/NL stock |
| ZetServers / X-ZoneServers | €659/mo | Dual Xeon E5-2630v3, 128 GB DDR4 | Guaranteed, unmetered | 13 locations; 50 Tbps+ network edge; AI-optimized routing included |

The ZetServers and X-ZoneServers lineups deserve their own table, since both providers publish the same seven guaranteed-unmetered configurations at identical prices:

| CPU | RAM | Storage | Price/mo |
| --- | --- | --- | --- |
| 2× Intel Xeon E5-2630v3 — 16c/32t @ 2.4 GHz | 128 GB DDR4-2133 | 6× SSD slots | €659 |
| 2× Intel Xeon E5-2690v4 — 28c/56t @ 2.6 GHz | 128 GB DDR4-2133 | 6× SSD slots | €679 |
| 2× Intel Xeon E5-2699v4 — 44c/88t @ 2.2 GHz | 256 GB DDR4-2133 | 6× SSD slots | €749 |
| 1× AMD EPYC 9254 — 24c/48t @ 2.9 GHz | 128 GB DDR5-4800 | 10× NVMe slots | €1,169 |
| 2× AMD EPYC 9254 — 48c/96t @ 2.9 GHz | 256 GB DDR5-4800 | 10× NVMe slots | €1,399 |
| 2× AMD EPYC 9554 — 128c/256t @ 3.1 GHz | 512 GB DDR5-4800 | 24× NVMe slots | €1,679 |
| 2× AMD EPYC 9754 — 256c/512t @ 2.3 GHz | 1 TB DDR5-4800 | 24× NVMe slots | €2,239 |

Locations span Bucharest, Dublin, Milan, Vienna, Frankfurt, Amsterdam, Sofia, Madrid, Lisbon, Paris, Marseille, Warsaw and Ashburn. Both providers state most in-stock configs activate within an hour, with OS installs taking 15–20 minutes; ZetServers adds a price-match offer and a 24/7 NOC with a 15-minute average ticket response. One substitution note from ZetServers itself: where the E5-2690v4 is unavailable, a similar config may ship with E5-2680v3 or E5-2670v3 instead — near-identical performance by their account, but worth knowing if core counts matter to your licensing.

## Limitations that don't make the headline

Four details separate plans that behave from plans that disappoint, and none of them appear in the price columns.

**Port wording.** Unihost's own explanation is the useful one here: the "10Gbps" label refers to the physical uplink between your server and the datacenter switch, and guarantees vary — some providers dedicate the full capacity, others sell burstable shared links. ZetServers and X-ZoneServers explicitly say "guaranteed"; Unihost claims non-oversubscribed redundant uplinks. If a plan page says neither "guaranteed" nor "non-oversubscribed", assume the port is shared until proven otherwise.

**Storage.** SATA SSDs top out around 500 MB/s, less than half of what a 10 Gbps pipe needs; NVMe reads exceed 3 GB/s, per Unihost. Concretely: Evoluso's €135 ST-2 ships a 500 GB SATA SSD, and Unihost's cheapest 10 Gbps config ships 8 TB HDDs. Both are fine for cold file distribution or backup targets — hopeless as a saturated streaming origin.

**DDoS coverage.** High-bandwidth servers attract attacks, as Unihost puts it; they include upstream scrubbing that activates automatically during volumetric attacks at no extra charge. ZetServers lists Layer 3–7 DDoS protection as a paid option still marked "coming soon" — if your traffic profile invites attacks, that's a real gap in the meantime.

**Management and activation.** Evoluso is unmanaged by default (IPMI KVM included for self-service). Unihost bundles free basic management. Activation windows also swing: under an hour for in-stock ZetServers configs, but up to 2–4 weeks when stock runs thin.

## Do you actually need a dedicated 10 Gbps server?

The workloads where it earns its keep are the ones where sustained multi-gigabit egress is the normal state: CDN origins feeding several pull zones during live events, VOD catalogs serving thousands of concurrent viewers off NVMe, national-scale VPN exit pools, and file mirrors moving petabytes monthly. Unihost adds a useful framing: one 10 Gbps server can replace several smaller machines, simplifying bandwidth planning in the process.

The honest counterweight comes from X-ZoneServers' own product page: a 10 Gbps VPS wears the same port number on the label. What dedicated hardware buys is single tenancy — your own kernel, your own interrupt map, no neighbor competing for the same network card or waking the same cores. If you can name what that separation is worth (compliance isolation, latency-shape control for voice or game traffic, running your own hypervisor with your own NUMA and pinning policy), dedicated is the right instrument. If you can't, a fat-pipe VPS does the job for less.

One more thing worth knowing before you overbuy: on the ZetServers/X-ZoneServers chassis, the same hardware takes the port up to 200 Gbps. Starting at 10 Gbps doesn't strand the machine if your traffic outgrows it.

## The budget-first route: RackNerd

If the published EUR tiers above feel heavier than your project needs, RackNerd is the natural first stop — it's the provider linked throughout this guide, and it has built its name on promotional pricing and stock that rotates constantly. That rotation is also why this guide doesn't quote a static RackNerd table: whatever was listed when this was written may be gone by the time you read it.

Two checks on the current listing before you order: which uplink the specific server carries (1 Gbps or 10 Gbps), and how traffic is billed. Then hold the all-in monthly cost against the benchmarks above. If a RackNerd box covers your transfer volume for less than the verified table, that's your deal. If you need a contractually guaranteed 10 Gbps port with published specs today, the €659 tier is the bar to clear. 👉 [see what RackNerd currently has in stock](https://bit.ly/my-rn)

## 10 Gbps dedicated server FAQ

**How much does a 10gbps dedicated server cost?**
Anywhere from about $141/mo (quota-capped, plus setup) to €2,239/mo for a 256-core EPYC flagship. The verified reference points: €135 promo for full-unmetered 10 Gbps on older dual-Xeon hardware at Evoluso; €659–749 for guaranteed-unmetered dual-Xeon tiers at ZetServers/X-ZoneServers; €1,169 for a single-socket EPYC 9254 on DDR5/NVMe; around $1,108/mo for true flat-unmetered 10 Gbps on server.net's cheapest hardware; €1,099.90+ ex. VAT at Unihost. Budget-first buyers should 👉 [compare RackNerd's live dedicated server pricing](https://bit.ly/my-rn) against these numbers.

**Is the 10 Gbps port guaranteed or shared?**
It depends entirely on the provider, and the wording of the plan page is the tell. "Guaranteed" or "non-oversubscribed" means the capacity is committed to your machine; anything describing the port as "burstable" means you share the uplink's headroom with other tenants.

**10 Gbps VPS or dedicated server — which one?**
If your workload only needs raw transfer volume, the VPS is cheaper and the port label is identical. Choose dedicated when tenancy itself matters: contract requirements that data sits on untouched hardware, latency-sensitive services that can't tolerate a noisy neighbor, or plans to run your own virtualization layer where you control the NUMA layout and pinning.

## Which 10 Gbps plan should you order?

Match the plan to the traffic shape, and the decision mostly makes itself:

- **Cheapest possible unmetered entry, CPU generation irrelevant** → Evoluso ST-2 at €135 promo. Just don't plan to saturate the port from that 500 GB SATA SSD.
- **CDN origin or VOD platform where the origin must never bottleneck** → ZetServers/X-ZoneServers. The dual E5-2690v4 at €679 is the mid-tier pick; the single EPYC 9254 at €1,169 keeps everything in one NUMA node, which is the simplest topology if you pin threads.
- **Spiky traffic — backups, sync windows, bursty downloads** → server.net's P95 commits, where the top 5% of samples are free.
- **Budget leads and specs are flexible** → start with RackNerd, benchmark what's in stock against the tables above, and only escalate to the guaranteed tiers if the uplink wording doesn't hold up.

Before ordering anywhere, confirm four things in this order: the port guarantee wording, whether the storage can actually feed the workload, what DDoS coverage is included today (not "coming soon"), and the realistic activation window. If budget is the deciding factor, 👉 [open RackNerd's current dedicated server lineup](https://bit.ly/my-rn) and run those four checks against whatever's listed before you commit anywhere else.
