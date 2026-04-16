# Art Talks — Agent Guide

This file tells AI coding agents (Cursor, Claude, Codex, etc.) how to work in
this repo. It applies to the whole project.

## What this project is

A static **Coming Soon** landing page for **Art Talks**, built with
[Astro 5](https://astro.build) and deployed on [Vercel](https://vercel.com).
No backend, no database, no user accounts. Fast, static, SEO-friendly.

- Production URL: https://arttalks-web.vercel.app
- GitHub: https://github.com/leony-prog/arttalks-web
- Vercel project: `fernandaleonys-projects/arttalks-web`

## Stack & conventions

- **Astro 5** with `output: 'static'` and the `@astrojs/vercel` adapter.
- **TypeScript strict** (`astro/tsconfigs/strict`).
- **Vanilla CSS** scoped to `.astro` components (no Tailwind, no UI lib).
- **Fonts**: Fraunces (display, serif) + Inter (body). Loaded from Google Fonts.
- **Images**: use `astro:assets` `<Image />` from `src/assets/` for optimization.
  Use `public/` only for files that must keep their exact path (favicon, OG image).
- **No client JS frameworks.** Keep the site zero-JS where possible; small
  vanilla `<script>` blocks only when needed.

## Directory layout

```
src/
  assets/         # optimized by Astro (import, pass to <Image />)
  layouts/        # shared HTML shell (SEO, fonts, global CSS)
  pages/          # file-based routes — index.astro is the only page for now
public/           # passthrough files (favicon, logo.png for OG)
astro.config.mjs  # Astro + Vercel adapter config
```

## Local development

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # -> ./dist and .vercel/output
npm run preview  # preview prod build locally
```

Always run `npm run build` before opening a PR — the Vercel build must pass.

## Deployment — how it works

**Git-driven, zero-touch.** The Vercel project is connected to the GitHub
repo, so:

| Event                       | Result                                   |
| --------------------------- | ---------------------------------------- |
| Push to `main`              | Production deploy → `arttalks-web.vercel.app` |
| Push to any other branch    | Preview deploy with a unique URL         |
| Open a PR                   | Preview deploy + URL posted to the PR    |

**The preferred workflow for shipping any change:**

```bash
git checkout -b feat/short-description
# ...edit files...
npm run build                 # verify it builds
git add -A
git commit -m "feat: short imperative description"
git push -u origin HEAD
gh pr create --fill           # Vercel will comment with a preview URL
# after review / merging the PR, production auto-deploys
```

**Hotfix / direct-to-prod (use sparingly):**

```bash
git add -A && git commit -m "fix: …" && git push origin main
```

### Manual CLI deploys (escape hatch only)

```bash
npx vercel              # one-off preview deploy from local files
npx vercel --prod       # one-off prod deploy from local files (skips CI)
npx vercel ls           # list recent deployments
npx vercel inspect <url>
npx vercel logs <url>
```

Prefer git-driven deploys — they are reproducible and leave a trail.

### Environment variables

Set them in the Vercel dashboard (Project → Settings → Environment Variables)
or via CLI:

```bash
npx vercel env add MY_VAR production
npx vercel env pull .env.local    # sync to local for dev
```

Never commit `.env*` files — they're git-ignored.

## Best practices for agents working here

1. **Edit, don't re-scaffold.** The project is set up; don't run
   `npm create astro` again.
2. **Keep it static.** Don't add SSR, API routes, or databases without
   explicit user request. If you do, switch `output` to `'server'` in
   `astro.config.mjs`.
3. **Preserve performance.** Avoid heavy JS libs. Prefer CSS + tiny vanilla
   scripts. Run `npm run build` and eyeball bundle sizes.
4. **Accessibility.** Keep semantic HTML, `aria-live` for form feedback,
   `prefers-reduced-motion` guards, visible focus states.
5. **SEO.** Update `<title>`, meta description, and OG tags in
   `src/layouts/Layout.astro` when copy changes.
6. **Never commit secrets.** Use Vercel env vars. Check `.gitignore` covers
   `.env*`, `.vercel/`, `dist/`, `node_modules/`.
7. **Small commits, conventional messages.** `feat:`, `fix:`, `chore:`,
   `docs:`, `refactor:`, `style:`.
8. **Ship via PR → auto-preview → merge.** Don't force-push `main`.

## Common tasks — quick recipes

- **Change hero copy** → `src/pages/index.astro` (`.title`, `.tagline`).
- **Swap the logo** → replace `src/assets/logo.png` (keeps `<Image />`
  optimization) and `public/logo.png` (favicon + OG image).
- **Change colors** → CSS variables in `src/layouts/Layout.astro` (`:root`).
- **Add a page** → new file under `src/pages/` (e.g. `about.astro`).
- **Wire the email form to a provider** → swap the inline `<script>` in
  `src/pages/index.astro` to `fetch()` against Formspree / ConvertKit /
  Mailchimp. Store the endpoint in a Vercel env var.
- **Add a custom domain** → Vercel Dashboard → Project → Settings → Domains.
