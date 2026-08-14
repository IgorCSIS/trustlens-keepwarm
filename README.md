# trustlens-keepwarm

A tiny scheduled GitHub Action that pings the TrustLens backend's `/health`
every 10 minutes so Render's free tier doesn't sleep (it naps after ~15 min
idle, which causes a slow cold start on the next scan).

Public repo on purpose: public repos get unlimited free Actions minutes.
No secrets here — the health URL is a public endpoint.

- Ping target: `https://trustlens-backend-djrm.onrender.com/health`
- Schedule: every 10 minutes (`*/10 * * * *`)
- Run it manually any time: Actions tab → keep-warm → Run workflow.

Note: GitHub disables scheduled workflows after 60 days with no repo activity —
push any commit to re-arm it. Scheduled runs can also be delayed a few minutes
under load; that's fine for keep-warm.
