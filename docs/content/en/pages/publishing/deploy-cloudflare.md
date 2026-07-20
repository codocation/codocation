---
title: Deploy to Cloudflare Pages
---

Cloudflare Pages hosts static sites on a global CDN with a generous free tier. Codocation
builds your site and uploads it to a Pages project directly from the IDE.

## First deploy

1. Choose `Tools → Codocation → Deploy Site`. The "Set Up Deployment" dialog opens.
2. Pick the "Cloudflare Pages" card and click "Next".
3. Paste an API token (see below), then click "Test Connection". Codocation verifies the
   token, detects your account, and lists your Pages projects.
4. Pick an existing project or type a name to create a new one. Project names use lowercase
   letters, digits, and hyphens.
5. Click "Deploy". The site is built and uploaded; the notification links to the published
   site.

### Create the API token

1. Open the [API tokens page](https://dash.cloudflare.com/profile/api-tokens) in the
   Cloudflare dashboard and click "Create Token".
2. Create a custom token and set its permission to "Account", "Cloudflare Pages", and
   "Edit".
3. Copy the token and paste it into the setup dialog.

The token is stored in the IDE's password safe, never in your project files. Your Account ID
is written to `codocation.yml`: it is not a secret, so it is safe to commit.

## Deploys after setup

Once configured, the action reads `Deploy to Cloudflare Pages` and publishes directly. The
production site lives at `https://<project>.pages.dev`.

The saved configuration sits under `deploy:` in `codocation.yml`:

```yaml
deploy:
  default: cloudflare
  cloudflare:
    accountId: <32-character account id>
    project: my-docs
    branch: main
```

## Custom domain

Attach your own domain in the Cloudflare dashboard: open "Workers and Pages", select your
project, and add the domain under "Custom domains". Cloudflare provisions the certificate
automatically.

## Managing the connection

`Settings → Tools → Codocation` shows the deploy configuration. From there you can change
the target, sign out (the stored token is removed), or reset the deploy configuration; the
setup dialog runs again on the next deploy.
