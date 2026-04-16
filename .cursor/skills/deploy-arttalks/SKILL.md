---
name: Deploy Art Talks
description: Ship Art Talks web changes to production on Vercel. Use when the user asks to deploy, ship, push live, or update the Art Talks site.
---

# Deploy Art Talks to Vercel

The Art Talks site is a static Astro site deployed on Vercel, with the Vercel
project connected to GitHub (`leony-prog/arttalks-web`). Deployment is
git-driven.

## Preflight

Before deploying, always:

1. Ensure the working tree is clean: `git status`.
2. Verify the build passes locally:
   ```bash
   npm run build
   ```
3. If dependencies changed, confirm `package-lock.json` is committed.

## Preferred flow — PR + preview + merge

This is the default. It gives us a preview URL on every change, and only
merges to `main` ship to production.

```bash
git checkout -b <short-branch-name>
# ...make edits...
npm run build                           # sanity check
git add -A
git commit -m "<type>: <imperative summary>"
git push -u origin HEAD
gh pr create --fill
```

Vercel will comment on the PR with a unique preview URL like
`arttalks-web-<hash>-fernandaleonys-projects.vercel.app`. When the PR is
merged into `main`, production auto-deploys to
**https://arttalks-web.vercel.app**.

## Fast path — direct push to `main`

Only for trivial changes (copy tweaks, asset swaps) the user explicitly
approves shipping straight to prod.

```bash
git add -A
git commit -m "<type>: <imperative summary>"
git push origin main
```

This triggers a Vercel production deploy within ~30s.

## Manual CLI deploy — escape hatch

Use only if git integration is unavailable or the user wants to ship local
uncommitted changes.

```bash
npx vercel              # preview deploy
npx vercel --prod       # production deploy
```

CLI deploys bypass GitHub, so they leave no PR trail. Prefer git-driven.

## Verification

After a deploy, verify:

```bash
# wait a few seconds, then:
curl -sI https://arttalks-web.vercel.app | head -1    # expect HTTP/2 200
npx vercel ls arttalks-web --yes | head -5             # latest deployment
```

Report the production URL back to the user:
**https://arttalks-web.vercel.app**

## Environment variables

Never hardcode secrets. For new env vars:

```bash
npx vercel env add <NAME> production
npx vercel env add <NAME> preview
npx vercel env add <NAME> development
npx vercel env pull .env.local    # sync locally
```

## Rollback

```bash
npx vercel ls arttalks-web --yes          # find a previous good deployment
npx vercel promote <deployment-url>       # promote it to production
```

Or revert the commit on `main` and push — Vercel will deploy the reverted
state.

## Common failures

- **Build fails on Vercel but passes locally** → check Node version.
  The project targets Node 20+. `.nvmrc` or `engines` in `package.json`
  pins it.
- **404 on logo/OG image** → ensure both `public/logo.png` and
  `src/assets/logo.png` exist; `public/` is for OG/favicon (fixed URL),
  `src/assets/` is for `<Image />` optimization.
- **"Project not linked"** → run `npx vercel link --yes --project arttalks-web`.
