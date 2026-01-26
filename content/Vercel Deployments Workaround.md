---
title: Vercel Deployments Workaround
date: 2023-12-12
tags:
  - seed
enableToc: false
---

This is a workaround for deploying to Vercel if you have multiple collaborators on the same repository but you're on the hobby plan. Normally this isn't possible because Vercel makes you pay for the pro plan to have multiple collaborators.

1. **Get the Deploy Hook URL from Vercel:**
   - Navigate to Project Settings → Git → Deploy Hooks
   - Create a hook (e.g., "Auto Deploy")
   - Copy the generated URL

2. **Add it to GitHub:**
   - Go to Repository Settings → Webhooks → Add webhook
   - Paste the Vercel Deploy Hook URL
   - Set Content type to `application/json`
   - Select "Just the push event"
   - Enable SSL verification
   - Click Add webhook

Now your deployments will trigger automatically when you push to your repository, without needing Vercel team access or changing git config. Also doesn't require you to set up any fancy CI/CD, this takes less than 5 minutes.