---
title: Deploy to GitHub Pages
---

GitHub Pages serves a static site straight from a branch of your repository. Codocation
builds the site and pushes it to that branch from the IDE.

## First deploy

1. Choose `Tools → Codocation → Deploy Site`. The "Set Up Deployment" dialog opens.
2. Pick the "GitHub Pages" card and click "Connect...".
3. The "Sign in to GitHub" dialog shows a one-time code and opens your browser at GitHub's
   device sign-in page. Enter the code there and authorize Codocation.
4. Once authorized, the deploy runs: the site is built and pushed to the `gh-pages` branch.

The repository is detected from your project's git remote, and the access token is stored in
the IDE's password safe, never in your project files.

If the site does not appear after the first push, check the repository's "Settings", section
"Pages": the site must be configured to deploy from the `gh-pages` branch.

## Deploys after setup

Once configured, the action reads `Deploy to GitHub Pages` and publishes directly.

The saved configuration sits under `deploy:` in `codocation.yml`:

```yaml
deploy:
  default: gh-pages
  ghPages:
    branch: gh-pages
```

The repository stays auto-detected from the git remote unless you pin it with an explicit
`repo:` value.

## Managing the connection

`Settings → Tools → Codocation` shows the deploy configuration. From there you can change
the target, sign out (the stored token is removed), or reset the deploy configuration; the
setup dialog runs again on the next deploy.
