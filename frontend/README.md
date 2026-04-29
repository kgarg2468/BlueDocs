# BlueDocs Frontend

Next.js app for the BlueDocs map dashboard.

## Local Setup

```bash
bun install
cp .env.example .env.local
```

Set these values in `.env.local`:

- `NEXT_PUBLIC_MAPBOX_TOKEN`: Mapbox public token with access to Mapbox styles.
- `NEXT_PUBLIC_API_URL`: BlueDocs API URL. Use `https://bluedocs-api.onrender.com` for the hosted backend or `http://localhost:8000` for local backend development.
- `NEXT_PUBLIC_CONVEX_URL`: Convex deployment URL from `bun run convex:dev` or the Convex dashboard.

Then run:

```bash
bun run dev
```

## Simple Convex Auth

Authentication intentionally stays simple:

- Users create an account with email and password.
- Sign-up immediately creates a session.
- There is no email verification, no magic link, and no external auth provider.
- Passwords must be at least 8 characters.
- Saved projects are stored per signed-in Convex user.

The auth functions live in `convex/accounts.ts`, and the session/password helpers live in `convex/lib/auth.ts`.

## Vercel Redeploy

Create a new Vercel project from this repo and set the project root to `frontend`.

Set Vercel environment variables:

- `NEXT_PUBLIC_MAPBOX_TOKEN`: New Mapbox public token.
- `NEXT_PUBLIC_API_URL`: `https://bluedocs-api.onrender.com`.
- `CONVEX_DEPLOY_KEY`: Convex production deploy key for the new Convex deployment.

The Vercel build command is configured in `vercel.json`:

```bash
npx convex deploy --cmd-url-env-var-name NEXT_PUBLIC_CONVEX_URL --cmd 'bun run build'
```

That command deploys Convex functions first, injects `NEXT_PUBLIC_CONVEX_URL` for the frontend build, then runs the Next.js production build.

If email notifications are needed, set `RESEND_API_KEY` in the Convex deployment environment. Without it, notification sending is skipped.
