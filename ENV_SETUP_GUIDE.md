# Environment Variables Setup for Netlify & Vercel

Your frontend is deployed on **Netlify** and backend is deployed on **Vercel**. This guide shows you where to set each environment variable.

---

## Frontend Environment Variables (Netlify)

**Site:** https://interprepai.netlify.app  
**Vite prefix rule:** Frontend env vars must start with `VITE_` to be exposed to client-side code.

### Steps to set in Netlify:
1. Go to your **Netlify dashboard** → Select your site → **Site settings** → **Build & deploy** → **Environment**
2. Add the following environment variables:

| Variable | Value | Description |
|----------|-------|-------------|
| `VITE_GEMINI_API_KEY` | `your_gemini_api_key_here` | Get from https://aistudio.google.com/apikey |
| `VITE_API_URL` | (leave empty) | Frontend will use relative `/api/*` paths; Netlify redirect handles the rest |

**Note:** Do NOT set server-only secrets here (e.g., `GEMINI_API_KEY`, `MONGODB_URI`, `JWT_SECRET`).

---

## Backend Environment Variables (Vercel)

**Site:** https://interprep-backend.vercel.app  
**Backend host:** The Express server runs on Vercel as a serverless function.

### Steps to set in Vercel:
1. Go to your **Vercel dashboard** → Select your project → **Settings** → **Environment Variables**
2. Add the following environment variables for **Production**:

| Variable | Value | Description |
|----------|-------|-------------|
| `GEMINI_API_KEY` | `your_gemini_api_key_here` | **Same value as frontend's `VITE_GEMINI_API_KEY`** |
| `MONGODB_URI` | `mongodb+srv://username:password@cluster.mongodb.net/interprepai` | MongoDB Atlas connection string |
| `JWT_SECRET` | `your-secret-key-change-in-production` | Used to sign JWT tokens (change this!) |
| `JWT_EXPIRES_IN` | `7d` | JWT token expiration time |
| `JWT_REFRESH_SECRET` | `your-refresh-secret-key-change-in-production` | Used to sign refresh tokens (change this!) |
| `JWT_REFRESH_EXPIRES_IN` | `30d` | Refresh token expiration time |
| `PORT` | `3000` | (Vercel manages this; set to `3000` or leave unset) |
| `NODE_ENV` | `production` | Environment mode |
| `CORS_ORIGIN` | `https://interprepai.netlify.app` | Allow frontend domain |

---

## Quick Checklist

- [ ] Set `VITE_GEMINI_API_KEY` in Netlify
- [ ] Set all server vars in Vercel (especially `MONGODB_URI`, `JWT_SECRET`, `GEMINI_API_KEY`)
- [ ] Ensure both Netlify and Vercel use the **same** `GEMINI_API_KEY` value
- [ ] Redeploy both sites after setting env vars (usually automatic; check build logs)
- [ ] Test the login/signup flow on https://interprepai.netlify.app

---

## Troubleshooting 502 Errors

If you still see **502 Bad Gateway** errors:

1. **Check Vercel backend logs:**
   - Go to Vercel → Your project → **Deployments** → View deployment details → **Logs**
   - Look for MongoDB connection errors or missing env vars

2. **Verify MongoDB is accessible:**
   - Ensure your MongoDB Atlas IP whitelist includes `0.0.0.0/0` (or Vercel's IP range if available)

3. **Check CORS settings:**
   - Ensure `CORS_ORIGIN` on Vercel includes `https://interprepai.netlify.app`

4. **Test the backend directly:**
   - Open https://interprep-backend.vercel.app/api/health (or similar health endpoint if you have one)
   - Should return a successful response (not 502)

---

## Useful Links

- Netlify env vars: https://docs.netlify.com/environment-variables/overview/
- Vercel env vars: https://vercel.com/docs/environment-variables
- MongoDB Atlas: https://www.mongodb.com/cloud/atlas
- Gemini API: https://aistudio.google.com/

