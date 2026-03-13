# Deployment

bboxfinder.com is hosted on GitHub Pages from the `cugos/bboxfinder.com` repository.

## How it works

The site is served from the `gh-pages` branch. The `main` branch is the source of truth for development. Deployment is a manual step — merging to `main` does **not** automatically update the live site.

When an admin triggers a deploy, a GitHub Actions workflow pushes `main` to `gh-pages`. GitHub Pages then rebuilds the site automatically (usually within a minute).

## Who can deploy

Anyone with admin access to `cugos/bboxfinder.com`. The workflow will fail if triggered by a non-admin. To see who has admin access: **Settings → Collaborators and teams**.

## How to deploy

1. Make sure the changes you want to deploy have been merged to `main`
2. Go to **Actions → Deploy to GitHub Pages**
3. Click **Run workflow** → select branch `main` → **Run workflow**
4. Watch the run complete — a green check means the deploy succeeded
5. Verify at [bboxfinder.com](https://bboxfinder.com)

## Two workflows, one deploy

Each deployment involves two workflows running in sequence:

1. **Deploy to GitHub Pages** — triggered manually by an admin, pushes `main` → `gh-pages`
2. **pages-build-deployment** — triggered automatically by GitHub Pages, builds and publishes the `gh-pages` branch to the live site

Both must succeed for changes to appear at bboxfinder.com.

## What not to touch

- **`gh-pages` branch** — do not commit directly to this branch. It is overwritten on every deploy. Any changes made there will be lost.
- **`CNAME` file** — present on both `main` and `gh-pages`, it maps the custom domain `bboxfinder.com`. Do not delete or modify it.
- **`github-pages` environment** — created automatically by GitHub to track Pages deployments. Leave it alone.

## Troubleshooting

**Deploy succeeded but site looks wrong:** Check that your changes are actually on `main` (`git log origin/main`). If a PR was merged to the wrong branch, it won't be included.

**Workflow fails with a permissions error:** Two possible causes:

- A non-admin triggered the workflow — the admin check at the start of the workflow will catch this and exit early with an error message
- The `GITHUB_TOKEN` lacks `contents: write` — check **Settings → Actions → General → Workflow permissions** is set to "Read and write permissions"

**Pages build fails after workflow succeeds:** Check the `github-pages` environment under **Settings → Environments** for error details. A broken `CNAME` or invalid file is the usual cause.
