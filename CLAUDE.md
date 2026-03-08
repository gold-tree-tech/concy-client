# Project: Concy Client (Widget)

## Architecture
- Static HTML widget (`index.html`) deployed on Vercel
- Backend server: `http://104.225.141.38:8083`
- Vercel serverless functions in `api/` proxy requests to avoid mixed content (HTTPS → HTTP)
  - `api/chat.js` → proxies `/api/v1/chat`
  - `api/complaint.js` → proxies `/api/v1/complaint`
- `vercel.json` rewrites `/api/v1/*` to `/api/*` serverless functions

## Pre-push checklist (MUST verify before every git push)
1. **No direct HTTP backend calls from client**: All fetch calls in `index.html` must use relative paths (e.g., `${baseUrl}/chat`, `${baseUrl}/complaint`), never `http://104.225.141.38:...` directly. Direct HTTP calls cause mixed content errors on HTTPS Vercel deployment.
2. **New API endpoints need a proxy**: If a new backend endpoint is added, create a corresponding `api/*.js` serverless proxy and add a rewrite rule in `vercel.json`.
3. **backendUrl in CONCY_CONFIG**: The `backendUrl` in config is only used for local dev. Production traffic must always go through Vercel proxy.
