# Deploying the frontend to Netlify (Interprep.AI)

This file contains quick steps to deploy the frontend to Netlify and notes about environment variables and the backend.

1) What this config does
- Uses `npm run build` to build the Vite React app and publishes the `dist` directory.
- Provides a redirect rule that proxies `/api/*` to an external backend (replace `YOUR_BACKEND_URL`).

2) Environment variables (set in Netlify dashboard → Site settings → Build & deploy → Environment)
- `VITE_GEMINI_API_KEY` — client-side Gemini key (Vite requires `VITE_` prefix).
- `VITE_API_URL` — optional. If set, your frontend will call this full URL for API requests. If you keep the `/api/*` redirect pointing to your backend and want same-origin requests, leave this empty and use relative `/api` paths.

Note: Do NOT set server-only secrets (`GEMINI_API_KEY`, `MONGODB_URI`, `JWT_SECRET`, etc.) in Netlify — those belong on your backend host.

3) Recommended backend approach
- Easiest: Deploy your existing Express backend separately (Render, Vercel, or a VPS). Configure that host with server env vars (`GEMINI_API_KEY`, `MONGODB_URI`, `JWT_SECRET`, `CORS_ORIGIN`), then update the redirect `YOUR_BACKEND_URL` or set `VITE_API_URL` to point to it.
- Alternate: Convert backend endpoints to Netlify Functions (more work; not covered here).

4) Quick manual deploy using Netlify CLI

Install Netlify CLI (once):
```powershell
npm install -g netlify-cli
```

Build and deploy (example):
```powershell
npm run build
npx netlify deploy --dir=dist --prod
```

If you prefer to use Netlify's Git integration, connect the repository in the Netlify dashboard and set the Build command to `npm run build` and Publish directory to `dist`.

5) Verification
- After deploy, visit your Netlify site URL and exercise the app.
- If API calls fail, ensure either:
  - `VITE_API_URL` is set to your backend URL; OR
  - `netlify.toml` redirect `from = "/api/*"` has `to` pointing to the backend host and CORS on the backend allows the site origin.

6) Useful links
- Netlify docs: https://docs.netlify.com/
- Netlify CLI docs: https://docs.netlify.com/cli/overview/
