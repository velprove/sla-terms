# Cloud Provider SLA Terms

Open dataset of standard SLA credit schedules for the major cloud providers. AWS, Google Cloud, Azure, Cloudflare, and Vercel side by side. Machine-readable JSON, maintained as terms change.

**Live site:** https://velprove.github.io/sla-terms/

**Interactive calculator:** https://velprove.com/sla-credit-calculator

## What's in here

`data/providers.json` is the source of truth. For each provider it lists:

- Default contracted SLA percentage
- Tier-bucketed credit schedule (uptime range → credit owed as % of monthly fee)
- Credit cap (most providers cap below 100% even for total failure)
- Claim deadline (days)
- Whether the plan has an SLA at all (Vercel Hobby/Pro famously does not)
- Link to the provider's official SLA page
- Reality-check notes on the claim process

## Why this exists

When a vendor misses their SLA, three failure modes usually leave you with $0 in credits:

1. The plan has no SLA in the first place (Vercel Hobby/Pro, most free tiers)
2. The outage falls under a carve-out (scheduled maintenance, third-party issues)
3. You missed the claim deadline (most providers require filing within 30 days)

The data here helps you check what you're actually owed before you waste time filing.

## Schedules

| Provider | Default SLA | Credit cap | Claim deadline | Notes |
|---|---|---|---|---|
| **AWS** | 99.9% | 100% | 30 days | EC2/S3/RDS default schedule |
| **Google Cloud** | 99.99% | 50% | 30 days | Compute Engine / GCS default |
| **Azure** | 99.9% | 25% | 60 days | Most services |
| **Cloudflare** | 99.9% | 25% | 30 days | Workers / Pages (Pro / Business) |
| **Vercel (Hobby/Pro)** | none | 0% | n/a | No SLA exists |

See `data/providers.json` for full tier schedules and per-provider notes.

## Use it

### As JSON

```bash
curl https://velprove.github.io/sla-terms/data/providers.json
```

### Programmatically

```js
const r = await fetch("https://velprove.github.io/sla-terms/data/providers.json");
const d = await r.json();
console.log(d.providers.find(p => p.id === "aws").tiers);
```

### Interactively

Open https://velprove.com/sla-credit-calculator to plug in your contract terms and see what you're owed.

## Caveats

- **Not legal advice.** Different services within the same provider may have slightly different tiers (AWS S3 differs from EC2). Enterprise contracts often have custom terms. For any commercial claim, verify against the actual contract text you signed.
- **Maintenance:** terms drift. Each entry links to the provider's official SLA page. Open an issue or PR if you notice drift.
- **No-SLA providers:** Vercel Hobby/Pro is explicit. Many other consumer SaaS products have similar gaps. If your provider is not listed and you do not see a public SLA page, assume the answer is "no SLA, no credit."

## Contributing

PRs welcome for:
- Provider terms drift (verify against the linked official SLA page, include the date)
- New providers (must have a public SLA page; include the URL in `sourceUrl`)
- Schema improvements that do not break the existing field set

## License

- Data (`data/providers.json`): CC-BY-4.0
- Code (`index.html`, build scripts): MIT

## Maintainer

[Velprove](https://velprove.com) — uptime monitoring that watches whether your site or service actually hits its SLA. Free plan, no credit card.
