# GitHub Pages (Knowledge Graph dashboard)

The workflow [`.github/workflows/deploy-knowledge-graph.yml`](../.github/workflows/deploy-knowledge-graph.yml) publishes the pre-built Understand Anything dashboard from `.ua/dashboard-dist/` to GitHub Pages.

## One-time repository setup (required)

If CI fails at **`actions/configure-pages`** with **`Get Pages site failed` / `Not Found`**, the repository does not have a Pages site yet. The default Actions token **cannot** create that site; an admin must enable it once:

1. Open **Settings → Pages** for `cloudera-applied-ai/AI_Blueprint_Microservices-with-Ray`.
2. Under **Build and deployment**, set **Source** to **GitHub Actions** (not “Deploy from a branch”).
3. Re-run the failed workflow (or push a change under `.ua/`).

If **Source → GitHub Actions** is missing, an organization policy may block Pages. A GitHub org admin must allow Pages for this repository (or make the repo public, if your plan requires that).

Optional: store a PAT with `repo` (or Pages admin) scope as `PAGES_ADMIN_TOKEN` and set `enablement: true` on `configure-pages` — only needed if you want automation without using the Settings UI.

## Published URL

After a successful deploy, the site is served at:

**https://cloudera-applied-ai.github.io/AI_Blueprint_Microservices-with-Ray/**

The static bundle must be built with Vite `--base "/AI_Blueprint_Microservices-with-Ray/"` so asset paths match that URL (see comments in the workflow file).

## Rebuilding the dashboard locally

```bash
cd ~/.claude/plugins/cache/understand-anything/.../packages/dashboard
VITE_DEMO_MODE=true \
VITE_GRAPH_URL="https://cloudera-applied-ai.github.io/AI_Blueprint_Microservices-with-Ray/knowledge-graph.json" \
pnpm exec vite build --config vite.config.demo.ts \
  --base "/AI_Blueprint_Microservices-with-Ray/" \
  --outDir <repo>/.ua/dashboard-dist --emptyOutDir
```

Commit `.ua/dashboard-dist/` and `.ua/knowledge-graph.json`, then push to `main`.
