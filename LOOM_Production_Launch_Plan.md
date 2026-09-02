# LOOM — Production Launch Plan

LOOM is a multi-agent orchestration engine with four specialist agents (Warp · R&D, Weft · Code, Heddle · Workflow, Selvage · Creative) routed by a single dispatcher. This plan takes LOOM from its current GitHub Pages prototype to a production-ready launch.

---

## 0. The Fork in the Road: BYOK Tool or Hosted Product

Everything below branches on this one decision.

| | **Stay BYOK** (bring-your-own-key) | **Go Hosted** (LOOM holds the key) |
|---|---|---|
| Backend needed | No | Yes |
| Can charge for LOOM itself | No — only value-add, not API usage | Yes |
| API key liability | None (user's own key, stored client-side) | You must secure and meter it |
| Time to launch | Days | Weeks |
| Legal/privacy surface | Minimal | Requires ToS, Privacy Policy, billing terms |

**Recommendation for MVP:** Launch BYOK first. It validates whether people want LOOM's orchestration pattern at all, before you take on backend and billing complexity.

---

## 1. Production Hosting

- **If BYOK:** Keep GitHub Pages. It's free, stable, and sufficient for a static four-agent router.
- **If Hosted:** Move the API layer to Vercel or Railway (backend for key + metering); frontend can stay static or move alongside it.
- **Custom domain:** Register something short and ownable (e.g. `loom-agents.dev` or similar) and point it at whichever host you land on.

## 2. Continuous Deployment

Add a GitHub Actions workflow that deploys on push to `main`:

```yaml
name: Deploy LOOM
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

No build step needed for the current single-file static structure.

## 3. Monitoring

- **Sentry (free tier):** catch JS errors in the router's dispatch logic — if Warp/Weft/Heddle/Selvage fail silently, you want to know before a user reports it.
- **UptimeRobot:** only needed if a backend is added (Hosted path) — a down backend means every agent goes idle.

## 4. Payments & Legal — Hosted path only

- Stripe Billing for subscription access.
- Privacy Policy disclosing that task inputs are relayed to Anthropic's API — users should know their prompts leave LOOM's infrastructure.
- One-page Terms of Service covering acceptable use.

*(Skip entirely if staying BYOK — no money or centrally-held keys means minimal legal surface.)*

## 5. Go-to-Market

LOOM's audience is individual developers and small teams evaluating agent orchestration — not enterprise procurement. No pitch decks or CRM needed at this stage.

- Launch on Product Hunt with the visible reasoning trace as the hook — it's what differentiates LOOM from single-agent tools.
- Share the GitHub Pages link and demo video in dev communities: Hacker News, r/LocalLLaMA, X/Twitter AI circles.
- Let the four-agent framing ("one goal, four specialists") carry the positioning — it's already distinctive copy.

---

## Immediate Next Steps

1. Confirm BYOK vs Hosted.
2. Add the GitHub Actions workflow above.
3. Wire in Sentry.
4. Buy the domain.
5. Prepare the Product Hunt launch post + demo video.
