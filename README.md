# Art Talks — Web

A Coming Soon landing page for **Art Talks**, built with [Astro](https://astro.build) and deployed on [Vercel](https://vercel.com).

## Stack

- **Astro 5** — static site generator
- **@astrojs/vercel** — Vercel adapter
- **Vanilla CSS** — no runtime framework, fast first paint
- **Fraunces + Inter** — editorial typography

## Local development

```bash
npm install
npm run dev      # start dev server at http://localhost:4321
npm run build    # build for production -> ./dist
npm run preview  # preview the production build
```

## Deployment

This project is connected to Vercel and auto-deploys:

- **Production** — every push to `main` ships to production.
- **Preview** — every PR and non-`main` branch gets a unique preview URL.

Manual deploys (from the CLI):

```bash
npx vercel           # deploy a preview
npx vercel --prod    # deploy to production
```

## Project structure

```
.
├── public/              # static assets served as-is (favicon, logo.png)
├── src/
│   ├── assets/          # optimized assets (processed by Astro)
│   ├── layouts/         # shared HTML shell
│   └── pages/           # routes (index.astro = /)
├── astro.config.mjs
├── package.json
└── tsconfig.json
```

## License

© Art Talks. All rights reserved.
