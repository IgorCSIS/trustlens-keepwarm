<p align="center">
  <img src=".github/assets/banner.svg" alt="TrustLens Keep-warm: one cron, one curl. Every ten minutes a scheduled GitHub Action requests the backend health endpoint, because Render's free tier sleeps after fifteen minutes idle." width="880">
</p>

<p align="center">
  <a href="https://github.com/IgorCSIS/trustlens-keepwarm/actions/workflows/keepwarm.yml"><img src="https://img.shields.io/github/actions/workflow/status/IgorCSIS/trustlens-keepwarm/keepwarm.yml?label=keep-warm&labelColor=3E2230&color=FFD23F&style=flat-square" alt="Keep-warm workflow status"></a>
  <img src="https://img.shields.io/badge/schedule-every%2010%20minutes-FFD23F?labelColor=3E2230&style=flat-square" alt="Runs every ten minutes">
  <img src="https://img.shields.io/badge/secrets-none-FFD23F?labelColor=3E2230&style=flat-square" alt="No secrets">
  <img src="https://img.shields.io/badge/license-MIT-FFD23F?labelColor=3E2230&style=flat-square" alt="MIT licensed">
</p>

# trustlens-keepwarm: one cron that keeps the backend awake

A tiny scheduled GitHub Action that pings the TrustLens backend's `/health`
every 10 minutes so Render's free tier doesn't sleep (it naps after ~15 min
idle, which causes a slow cold start on the next scan).

Public repo on purpose: public repos get unlimited free Actions minutes.
No secrets here: the health URL is a public endpoint.

- Ping target: `https://trustlens-backend-djrm.onrender.com/health`
- Schedule: every 10 minutes (`*/10 * * * *`)
- Run it manually any time: Actions tab → keep-warm → Run workflow.

Note: GitHub disables scheduled workflows after 60 days with no repo activity.
Push any commit to re-arm it. Scheduled runs can also be delayed a few minutes
under load; that's fine for keep-warm.

## Where this sits

<p align="center">
  <img src=".github/assets/repos.svg" alt="The four TrustLens repositories. trustlens-web posts scan and report requests to trustlens-backend, which reads Base and an AI triage API. trustlens-keepwarm requests the backend health endpoint every ten minutes. trustlens-contracts holds PaymentGate on Base Sepolia, and both payment arrows are dashed because they are switched off while the scanner is in free beta." width="880">
</p>

This repository is the small box on the right. It exists for one reason, and
that reason is a line in somebody else's pricing page.

## The 60-day clock

This is the one thing that actually breaks. The cron does not count as repo
activity, so only a push re-arms it. Last re-armed 2026-09-19, which means
the schedule runs until roughly 2026-11-18 without another push.

If it lapses, nothing errors: the workflow simply stops running, Render sleeps
after 15 minutes idle, and the next scan pays a cold start. The fix is a push.

The durable fix is to move this workflow into trustlens-backend, where normal
development keeps the repo active on its own. It lives here only because
public repos get unlimited Actions minutes, and trustlens-backend is public
too, so that reason no longer holds.

## License

MIT. See [LICENSE](LICENSE).

Built by **Igor Lima**. Automation and web work for East County and San Diego
businesses. Portfolio: https://igorcsis.github.io/niftyai-portfolio/
