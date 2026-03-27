# Upptime Status Page Setup

Repo: **https://github.com/docaifabric/status**

---

## Maintenance Notes

- **Rotating the PAT**: When the token expires, regenerate it and update the `GH_PAT` secret
- **Adding new URLs**: Just add to the `sites:` list in `.upptimerc.yml` and push
- **Check interval**: Default is every 5 minutes (configured in `.github/workflows/uptime.yml`)
- **History**: All incident history is stored as GitHub Issues in the repo, publicly visible
