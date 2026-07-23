---
title: Deploy to Cloudflare Pages
---

Cloudflare Pages hosts static sites on a global CDN with a generous free tier. Codocation
builds your site and uploads it to a Pages project directly from the IDE.

## First deploy

1. Choose `Tools → Codocation → Deploy Site`, then choose "Set Up Deployment…".
2. Enter a deployment name, select one or more documentation sites, and choose
   "Cloudflare Pages".
3. Paste an API token (see below). Codocation verifies the token and resolves its account.
4. Select the target Pages project. Project names use lowercase letters, digits, and hyphens.
5. Click "Deploy". Codocation saves the named group, builds every selected site as one
   complete snapshot, and uploads it to the Pages project.

### Create the API token

1. Open the [API tokens page](https://dash.cloudflare.com/profile/api-tokens) in the
   Cloudflare dashboard and click "Create Token".
2. Create a custom token and set its permission to "Account", "Cloudflare Pages", and
   "Edit".
3. Copy the token and paste it into the setup dialog.

The token is stored in the IDE's password safe, never in project files. The non-secret Account ID
and Pages project are written to `deployments.yml` and are safe to commit.

## Deploys after setup

The Deploy action lists saved groups in file order. Selecting a group opens its confirmation
dialog without network activity. Use "Deploy" for unchanged membership or "Update and Deploy"
after changing its selected sites. "Deploy All…" preflights every selected group before the first
upload and then runs independent uploads with bounded concurrency.

The saved configuration lives in project-root `deployments.yml`:

```yaml
deployments:
  production:
    name: Production documentation
    sites:
      - docs
      - api
    provider: cloudflare-pages
    accountId: <32-character account id>
    project: my-docs
```

## Custom domain

Attach your own domain in the Cloudflare dashboard: open "Workers and Pages", select your
project, and add the domain under "Custom domains". Cloudflare provisions the certificate
automatically.

## Managing the connection

`Settings → Tools → Codocation` shows all deployment groups with their sites, provider,
destination, and credential status. Add, edit, or remove groups there. Removing a group changes
local configuration only; it does not delete a Pages project or remote content.
