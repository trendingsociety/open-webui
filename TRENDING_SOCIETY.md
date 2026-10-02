# Trending Society fork of Open WebUI

Branch `trendingsociety` tracks an upstream release tag plus one patch: branding read from config.

## The patch
`backend/open_webui/env.py` reads the settings below; `backend/open_webui/config.py` applies the brand assets right after it clears and refills `static/` at startup.

| Variable | Effect |
| --- | --- |
| `BRANDING_OVERRIDE_UNDER_50_USERS=true` | Drops the " (Open WebUI)" suffix from `WEBUI_NAME` and enables the two settings below |
| `BRAND_ASSETS_DIR=/app/brand` | At startup, copies every file in that directory over the same-named file in `static/` (`favicon.png`, `logo.png`, `splash.png`, `custom.css`, ...) |
| `BRAND_FAVICON_URL` | Replaces the remote favicon URL |

LICENSE section 4 allows replacing Open WebUI branding only for deployments under 50 end users in any rolling 30-day period, with written permission, or under an enterprise licence. Leave `BRANDING_OVERRIDE_UNDER_50_USERS` unset on any larger deployment.

## Building
`Dockerfile.branded` layers the patched `env.py` and `config.py` on `ghcr.io/open-webui/open-webui:<tag>`. Frontend features need the full upstream `Dockerfile` instead.

## Upgrading
```bash
cd ~/Documents/GitHub/open-webui && git fetch upstream --tags && git merge <new-tag>
```
Then set `OPEN_WEBUI_TAG` in `Dockerfile.branded` to the same tag and rebuild.
