# E.K. Tetteh Library — Vercel deployment

## What was changed
- Corrected the hero slideshow image paths to match the `.JPG` filenames (Linux/Vercel paths are case-sensitive).
- Added a Vercel serverless entry point at `api/[...path].js` for the existing Express API.
- Added a root `package.json` so Vercel installs Express and rss-parser when the project root is deployed.
- Changed the backend to export its Express app for Vercel while retaining `npm start` for local hosting.
- Made the Graphic Online feed display available without an OpenAI key. Without a key, cards use the publisher-provided description instead of an AI-generated summary.

## Deploy
1. Upload the contents of this folder to the GitHub repository connected to Vercel. The `index.html`, `app.js`, `styles.css`, `assets/`, `api/`, `backend/`, `package.json`, and `vercel.json` must be at the repository root (not inside an extra nested project folder).
2. In Vercel, open the project and confirm **Root Directory** is the folder containing `index.html` and the root `package.json`.
3. Add these Environment Variables in Vercel Project Settings → Environment Variables:
   - `GRAPHIC_RSS_URL` = the RSS/Atom feed URL that Graphic Online permits your project to use.
   - `OPENAI_API_KEY` = your own OpenAI API key (optional; required for AI-generated summaries).
   - `OPENAI_MODEL` = `gpt-4o-mini` (optional; default is `gpt-4o-mini`).
   - `REFRESH_MINUTES` = `30` (optional).
4. Redeploy the project.
5. Test `https://YOUR-VERCEL-DOMAIN/api/health`. It should return JSON showing `ok: true`.
6. Test `https://YOUR-VERCEL-DOMAIN/api/graphic-news`. If the feed URL is valid and permitted, it should return an `items` array.

## Important
- Do not put the OpenAI key in `app.js`, HTML, or any public file. Add it only as a Vercel Environment Variable.
- An OpenAI API key is separate from a ChatGPT subscription and may require API billing/credits.
- No API key means headlines can still load, but AI-generated summaries will not. The cards will identify their text as a publisher description.
- The feed URL in `.env.example` is only an example from the project; confirm the correct permitted feed URL with Graphic Online before relying on it.
- Vercel Hobby is intended for personal, non-commercial projects. Check current Vercel plan terms for your deployment use.
- This project currently stores several library records in browser local storage / IndexedDB. Those records are local to each browser and are not a shared cloud database. This deployment fix does not add persistent shared storage or production authentication.
