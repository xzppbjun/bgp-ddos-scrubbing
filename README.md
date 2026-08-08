# BGP DDoS Protection: Always-On Scrubbing Without Ripping Out Your Network, 100Gbps Mitigation From $39/IP

If you've ever stared at a monitoring dashboard while a flood of garbage traffic swallowed your bandwidth whole, you already know why "BGP DDoS protection" gets typed into search bars at 3 a.m. It's the cleanest answer to a dirty problem: instead of buying bigger pipes or stacking appliances in your rack, you let someone else's scrubbing network absorb the hit, and BGP is the lever that flips the traffic over. This piece walks through how that actually works, where the technology is heading in 2026, and how one provider that's been doing this since 2003 — Sharktech — turns it into something you can buy without a six-figure procurement cycle.

## Why BGP-Based DDoS Scrubbing Became the Default Play

The old way of fighting DDoS was brute force: buy more bandwidth than the attacker, install dedicated mitigation boxes, and pray your upstream doesn't null-route you first. It works, sort of, until you price out the hardware, the transit commitments, and the network engineer who actually knows how to tune it. Most teams clock the bill in the hundreds of thousands and quietly look for an exit.

BGP-based scrubbing flips the model. You peer a BGP session with a protection provider, announce your prefixes to them, and they announce you to the internet. Inbound traffic flows through their scrubbing centers, gets cleaned, and comes back to you — typically over a GRE tunnel so only ingress is rerouted and latency impact is halved. When an attack lands, their firewalls filter the malicious flows in real time; clean traffic keeps moving. No new hardware in your rack, no software agent on your boxes, no migration. You keep your IPs, your upstream, your topology.

Industry adoption has been climbing steadily. A five-year longitudinal analysis published by APNIC in late 2025 confirms that BGP-based DDoS scrubbing is no longer a niche ISP trick — it's the go-to method for AS operators who want protection without a forklift upgrade. The technology has matured to the point where on-demand rerouting, always-on filtration, and BGP Flowspec rules for granular filtering are all standard tools in the kit.

## What Actually Matters When You're Shopping for BGP DDoS Protection

Before we get to specific plans, here's the shortlist of questions worth asking any provider, because the marketing pages all sound the same until you dig in:

- **Mitigation capacity.** Not your plan's cap — the provider's total absorbable volume. A 60Gbps plan is useless against a 200Gbps attack if the scrubbing network itself only has 80Gbps of headroom.
- **Always-on vs. on-demand.** Always-on means traffic is always routed through the scrubber (cleaner, slight latency). On-demand means traffic reroutes only when an attack is detected (lower baseline latency, depends on fast detection).
- **Hardware/software requirements.** The whole point of BGP-based protection is that you shouldn't need to buy anything. If a vendor's "BGP protection" still ships you an appliance, keep looking.
- **Prefix requirements.** Most BGP scrubbing setups need you to own at least a /24. If you only have a /25 carved out of someone else's block, that limits your options.
- **Attack types covered.** Volumetric floods are table stakes. The serious ones also handle TCP state-exhaustion, application-layer HTTP floods, and amplification/reflection attacks (DNS, NTP, Memcached, SSDP, SNMP, Chargen).
- **Pricing model.** Per-IP, per-Gbps, flat-rate, or "free with hosting." The difference between $39/IP and $200/IP adds up fast across a few dozen prefixes.

Keep that list in your head and the rest of this falls into place pretty naturally.

## Where Sharktech Fits In

Sharktech is a Las Vegas-based infrastructure provider that started life in 2003 as a DDoS protection company and grew into full-stack hosting. Today they run five data centers — Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam — connected to Tier-1 carriers including Comcast, Tata, GTT, China Telecom, and China Mobile. Their global connectivity sits at around 1.1Tbps after a recent round of router upgrades with 100Gbps uplinks, which is the kind of capacity that makes "we have yet to receive an attack we couldn't mitigate" a believable claim rather than a tagline.

The thing that makes them relevant to a BGP DDoS protection search is that BGP isn't a bolt-on feature for them — it's the primary delivery mechanism for their Remote Network DDoS Protection product. If you want to read the technical breakdown straight from the source, you can 👉 [explore Sharktech's Remote Network DDoS Protection](https://bit.ly/SharKTech).

### How Their BGP Scrubbing Actually Works

The mechanics are textbook remote BGP scrubbing, executed cleanly:

1. **BGP session up.** An external BGP session is established between your network (or soft router — no dedicated hardware required) and Sharktech's routers.
2. **Prefix announcement.** You hand them your prefix list (minimum /24), they announce your network to the internet.
3. **GRE tunnel back.** Clean traffic returns to you over a GRE tunnel. Only ingress is routed through them, which cuts the latency penalty in half compared to symmetric rerouting.
4. **Attack detection and filtering.** When their systems detect an attack, the targeted destination is rerouted to on-site firewalls. Malicious traffic is filtered; legitimate traffic continues flowing back through the GRE tunnel.

The requirements are reasonable: a /24 or larger assigned to your company, a system that can do BGP and GRE (a soft router is fine), and ideally an MTU of at least 1550 with your upstream to absorb GRE overhead. That's it. No forklift, no appliance, no migration.

### What It Defends Against

Sharktech publishes a concrete list of attack types their mitigation handles, and it covers the categories you'd actually expect to get hit with:

- **Volumetric:** UDP Flood, ICMP Flood, ACK Flood, Ping of Death, Smurf, ICMP + UDP Flood
- **Amplification/Reflection:** NTP, DNS, SSDP, SNMP, Chargen, MemCached, Reflected ICMP & UDP
- **TCP state-exhaustion:** TCP SYN Flood, SYN-ACK-ACK
- **Application-layer:** HTTP Flood, HTTP POST Flood, Slowloris
- **Misc:** NXDomain

That's not a marketing "and more" list — it's a specific, testable inventory. If you've been on the receiving end of a Memcached amplification attack that doubled your inbound every 30 seconds, knowing a provider explicitly names it is reassuring.

## The Plan Landscape: From Free-With-Hosting to 100Gbps Add-Ons

Sharktech's DDoS protection pricing splits into two lanes, and understanding the split is the key to not overpaying.

**Lane 1 — DDoS protection included with hosted services.** Every VPS, bare-metal dedicated server, cloud instance, and colocation deployment comes with their proprietary DDoS protection baked in at 60Gbps baseline. No line item, no per-IP fee. If you're already in the market for infrastructure, the protection is effectively free.

**Lane 2 — DDoS protection as a standalone or upgraded product.** This is where BGP DDoS protection becomes the actual product:

- **100Gbps DDoS protection add-on:** $39/month per single IP. Can be attached to any dedicated or colocated server. This is a recent price reduction Sharktech announced after their capacity upgrades, explicitly aimed at making 100Gbps mitigation accessible to more clients.
- **Remote Network DDoS Protection:** For networks you own and host elsewhere. Pricing is custom-quoted based on your prefix count and capacity needs, which is standard for BGP scrubbing at this tier — Azure's Network Protection, for comparison, runs around $2,944/month plus per-resource fees, so the category isn't cheap, but Sharktech's per-IP add-on model is dramatically more accessible for smaller deployments.

If you want to see which lane fits your situation and pull live pricing, you can 👉 [check current Sharktech plans and DDoS add-ons](https://bit.ly/SharKTech).

### Smart VPS Plans (DDoS Protection Included)

These are the entry point for most people. All plans include 60Gbps DDoS protection, 10Gbps port speed, Xeon Gold CPUs, enterprise NVMe storage, 24/7 human support, and deployment across LA, Denver, Chicago, or Amsterdam.

| Plan | vCPU | RAM | NVMe | Bandwidth | Monthly | Annual (50% off) | Get Started |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tiny | 1 core | 2 GB | 40 GB | 4 TB | $7.95/mo | $3.98/mo | [Order Tiny](https://bit.ly/SharKTech) |
| Small | 2 cores | 4 GB | 80 GB | 8 TB | $15.95/mo | $7.98/mo | [Order Small](https://bit.ly/SharKTech) |
| Medium | 2 cores | 8 GB | 160 GB | 16 TB | $31.95/mo | $15.98/mo | [Order Medium](https://bit.ly/SharKTech) |
| Large | 4 cores | 16 GB | 320 GB | 32 TB | $63.95/mo | $31.98/mo | [Order Large](https://bit.ly/SharKTech) |
| XL | 4 cores | 32 GB | 640 GB | 64 TB | $127.95/mo | $63.98/mo | [Order XL](https://bit.ly/SharKTech) |

The annual discount is automatic — no coupon needed. At $3.98/month for a DDoS-protected Tiny VPS, the math gets hard to argue with if you need a protected workload online fast.

### DDoS Protection Add-On Pricing

| Option | Protection Level | Pricing | Best For | Link |
| --- | --- | --- | --- | --- |
| Standard (included) | 60Gbps | Free with all hosted services | VPS, dedicated, cloud customers | [Included with hosting](https://bit.ly/SharKTech) |
| 100Gbps add-on | 100Gbps | $39/month per single IP | Dedicated/colocation servers facing large volumetric attacks | [Add 100Gbps protection](https://bit.ly/SharKTech) |
| Remote Network DDoS Protection | Up to provider's full 1.1Tbps capacity | Custom quote | Networks you own but host elsewhere; ISPs, data centers, hosting providers | [Request remote protection quote](https://bit.ly/SharKTech) |

## Active Promo Codes and Discounts

Sharktech doesn't run constant flash sales, which is actually a good sign — the discount structure is predictable and the recurring codes apply every billing cycle, not just the first month.

- **Annual billing:** 50% off automatically on Smart VPS (best value)
- **Semi-annual billing:** 35% off automatically
- **Quarterly billing:** 25% off automatically
- **Promo code `Y5YET1Z9EK`:** 10% recurring discount on Cloud Virtual Servers and Bare Metal Dedicated Servers — also unlocks 20% off for Amsterdam-specific deployments
- **Promo code `WHTFALL`:** 33% recurring discount on Cloud Virtual Data Center services

The recurring nature of `Y5YET1Z9EK` is the detail worth pausing on. It's not a honeymoon discount that vanishes after month one; it applies every cycle for as long as you're a customer. If you're deploying in Amsterdam, the 20% stack is the better play. You can 👉 [apply a promo code at checkout](https://bit.ly/SharKTech) when you sign up.

## What Real Users Say

The review pattern that shows up consistently across LowEndTalk, Trustpilot, and long-tenure customer testimonials is less "wow, amazing features" and more "the bad thing didn't happen." No bandwidth overage bills. No surprise suspensions when an attack landed. No throttling hidden in the fine print.

A one-year review on LowEndTalk from a customer who'd been actively targeted put it plainly: "Sharktech successfully stopped the DDoS attacks. I was pleased." Game server operators — the demographic that treats incoming DDoS as a daily operational reality rather than an edge case — are the most vocal segment. Dingdian Network Co. noted their game servers routinely absorb 3–8Gbps attacks without skipping a beat. Kill-Streak Gaming, a mainland China IDC, has been with Sharktech for years and describes them as "totally trustworthy."

The less-glowing feedback is consistent too: no money-back guarantee, all payments non-refundable, knowledge base is thin. If you're used to managed shared hosting with a 30-day refund window, that's a culture shift. If you're an operator who already knows their way around a terminal, it's a non-issue. You can 👉 [read more customer reviews and sign up](https://bit.ly/SharKTech).

## Who This Actually Fits

**Good fit:**

- Game server operators who get hit with DDoS attacks as a regular cost of doing business
- ISPs, data centers, and hosting providers who need BGP-based scrubbing for their own AS without building it in-house
- Businesses running their own infrastructure elsewhere that want remote protection bolted on via BGP — no migration, no IP changes
- Teams migrating off hyperscalers (AWS, Azure, GCP) looking for predictable pricing and infrastructure-level DDoS protection that doesn't bill per-resource
- Anyone deploying in the US or Amsterdam with latency-sensitive workloads

**Probably not the right fit:**

- Beginners who want click-to-deploy managed WordPress and a refund window
- Projects where the workload would be fine on cheap shared hosting and DDoS isn't actually a concern
- Teams that need a fully managed application layer — Sharktech is unmanaged infrastructure; their Cloud Application Platform covers some of this, but the core services assume technical comfort

## The Bottom Line

BGP DDoS protection works because it solves the right problem at the right layer — you don't need bigger pipes or more appliances, you need someone else's scrubbing network and a BGP session to redirect traffic through it. The technology has matured to the point where on-demand and always-on rerouting are both viable, Flowspec rules enable granular filtering, and providers with serious backbone capacity can absorb attacks that would flatline a self-built defense.

Sharktech's angle is that they've been doing this for over 20 years, their 1.1Tbps global capacity is real, their BGP/GRE remote protection requires no hardware on your end, and their pricing model — 60Gbps included free with hosting, 100Gbps at $39/IP, remote protection custom-quoted — is one of the more accessible on-ramps in a category where the hyperscaler alternatives run into the thousands per month. The annual VPS discount alone puts a DDoS-protected server at under $4/month, which is hard to beat for what you're getting.

If your work matches what they're built for, there aren't many providers doing it better. You can 👉 [get started with Sharktech's BGP DDoS protection](https://bit.ly/SharKTech) and see the current plans, add-ons, and promo codes directly.
