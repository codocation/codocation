---
title: Deploy to GitHub Pages
---

GitHub Pages serves a static site straight from a branch of your repository. Codocation
builds the site and pushes it to that branch from the IDE.

## First deploy

1. Choose `Tools → Codocation → Deploy Site`, then choose "Set Up Deployment…".
2. Enter a deployment name, select one or more documentation sites, and choose "GitHub Pages".
3. Enter the repository, branch, and optional publish path, then click "Connect…".
4. The "Sign in to GitHub" dialog shows a one-time code and opens your browser at GitHub's
   device sign-in page. Enter the code there and authorize Codocation.
5. Click "Deploy". Codocation saves the named group, builds every selected site as one complete
   snapshot, and pushes it to the configured branch and path.

The access token is stored in the IDE's password safe, never in project files.

If the site does not appear after the first push, check the repository's "Settings", section
"Pages": the site must be configured to deploy from the `gh-pages` branch.

## Deploys after setup

The Deploy action lists saved groups in file order. Selecting a group opens its confirmation
dialog without network activity. Use "Deploy" for unchanged membership or "Update and Deploy"
after changing its selected sites. "Deploy All…" preflights every selected group before the first
upload and then runs independent uploads with bounded concurrency.

The saved configuration lives in project-root `deployments.yml`:

```yaml
deployments:
  github-mirror:
    name: GitHub mirror
    sites:
      - docs
    provider: github-pages
    repository: company/documentation
    branch: gh-pages
    path: docs/
```

Omit `path` to publish at the branch root.

## Managing the connection

`Settings → Tools → Codocation` shows all deployment groups with their sites, provider,
destination, and credential status. Add, edit, or remove groups there. Removing a group changes
local configuration only; it does not delete a repository, branch, or remote content.
