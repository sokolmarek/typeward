# Typeward Docs

Documentation site for Typeward, the open-source LaTeX and Typst editor ([source and releases](https://github.com/sokolmarek/typeward), MIT with the Commons Clause). Built with [Astro Starlight](https://starlight.astro.build). Deployed to https://typeward.com via GitHub Pages on every push to `main` that touches `docs/` (`.github/workflows/docs-deploy.yml` at the repository root; `docs-ci.yml` checks pull requests).

## Working on the docs

The site is its own npm project, separate from the app. Run these from this `docs/` folder:

```
npm install
npm run dev       # local preview at localhost:4321
npm run build     # production build + internal link validation
```

Pages live in `src/content/docs/` as Markdown. The sidebar is explicit in `astro.config.mjs` -- only finished pages are listed; no "coming soon" stubs. The build fails on broken internal links (`starlight-links-validator`).

[`STYLE.md`](STYLE.md) is the writing guide: house rules, a skeleton per page type, the terminology table, and a checklist to run against a finished page.
