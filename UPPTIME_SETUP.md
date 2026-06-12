# Upptime Status Page Setup

Repo: **https://github.com/docaifabric/status**

---

## Subscriber notifications (public subscribe/unsubscribe)

Anyone can subscribe on the status page to receive an email when an incident starts
or is resolved. This is built from two pieces, since Upptime has no native
subscriber feature:

1. **Subscribe form** — embedded in the status page via `customFootHtml` in
   `.upptimerc.yml`. It posts to [Buttondown](https://buttondown.com), which handles
   double opt-in confirmation and puts an unsubscribe link in every email.
2. **Incident emails** — `.github/workflows/notify-subscribers.yml` fires when an
   Upptime incident issue is opened or closed (issues labeled with a site slug,
   e.g. `doc-ai-fabric-production`) and sends a broadcast to all subscribers via
   the Buttondown API.

### One-time setup

1. Create a free Buttondown account (free tier: up to 100 subscribers) with
   username **docaifabric-status** — or pick another and update the form action in
   `.upptimerc.yml` (`embed-subscribe/<username>`).
2. In Buttondown: Settings → API → copy the API key.
3. Add it as a repository secret named `BUTTONDOWN_API_KEY`
   (Settings → Secrets and variables → Actions → Secrets).

Without the secret, the workflow exits cleanly without sending.

### Caveats

- The notify workflow only triggers if incident issues are created with the
  `GH_PAT` token. If `GH_PAT` expires and Upptime falls back to the default
  `github.token`, GitHub suppresses downstream workflow triggers and no
  subscriber email goes out — one more reason to keep the PAT fresh.
- A `notifications:` block in `.upptimerc.yml` has **no effect** in Upptime
  (an earlier Telegram config was dead for this reason). For internal ops alerts,
  Upptime reads `NOTIFICATION_*` repository secrets — see
  https://upptime.js.org/docs/notifications — but the subscriber flow above does
  not use them.

### Testing

Temporarily point a site entry at a non-existent URL, manually dispatch the
**Uptime CI** workflow, and confirm a subscribed address receives the incident
email (and the resolution email after reverting).

## Maintenance Notes

- **Rotating the PAT**: When the token expires, regenerate it and update the `GH_PAT` secret
- **Adding new URLs**: Just add to the `sites:` list in `.upptimerc.yml` and push
- **Check interval**: Default is every 5 minutes (configured in `.github/workflows/uptime.yml`)
- **History**: All incident history is stored as GitHub Issues in the repo, publicly visible
