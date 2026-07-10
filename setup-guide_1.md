# Google Ads AI Connector — Setup Guide

This toolkit lets Claude read your Google Ads campaign performance and draft new ad creative (Responsive Search Ads) directly in your account — **paused by default**, so nothing goes live or spends money without you clicking "enable" yourself.

Total setup time: ~20–30 minutes, one time only.

---

## What's in this bundle

- `google-ads-connector.json` — the connector spec (tells Claude how to talk to Google Ads)
- `setup-guide.md` — this document
- Example prompts (see bottom of this guide)

---

## Part 1: Get Google Ads API access

### Step 1 — Create or confirm your Google Ads account
If you don't already have one, create one at [ads.google.com](https://ads.google.com). A free account works for setup and testing — you don't need active campaigns yet.

### Step 2 — Request a developer token
1. In Google Ads, go to **Tools & Settings → Setup → API Center**
2. Apply for a developer token
3. You'll get **instant access at "Test account" level** — this is enough to try everything in this guide
4. If you later want to manage a real, live account, you'll need to apply for **Standard access**, which Google reviews manually (can take days to weeks). You don't need this to start.

### Step 3 — Create OAuth credentials
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project (or use an existing one)
3. In the search bar, find and enable **"Google Ads API"**
4. Go to **APIs & Services → Credentials → Create Credentials → OAuth client ID**
5. Choose **"Desktop app"** as the application type
6. Save the **Client ID** and **Client Secret** shown — you'll need these next

### Step 4 — Get your access token
1. Go to [Google OAuth Playground](https://developers.google.com/oauthplayground)
2. Click the gear icon (top right) → check **"Use your own OAuth credentials"** → paste in your Client ID and Client Secret from Step 3
3. In the left panel, find and select the scope: `https://www.googleapis.com/auth/adwords`
4. Click **Authorize APIs** → sign in with your Google Ads account → allow access
5. Click **Exchange authorization code for tokens**
6. Copy the **Access token** shown — this is what you'll paste into Claude

> Access tokens expire after about an hour. If Claude reports an auth error after a while, repeat Step 4 to get a fresh token.

---

## Part 2: Connect to Claude

1. Open Claude → **Settings → Connectors**
2. Choose **Add custom connector**
3. Upload or paste in the contents of `google-ads-connector.json`
4. When prompted for credentials, enter:
   - **developer-token**: from Part 1, Step 2
   - **Authorization**: your access token from Part 1, Step 4
5. Save

You're set up.

---

## Part 3: Using it

### Reading performance data
Ask Claude things like:

> "Check my Google Ads account [customer ID] and show me which campaigns have the highest cost per conversion."

> "Pull impressions and clicks for my enabled campaigns this month."

### Generating and posting new ad creative
Ask Claude things like:

> "Find my worst-performing ad group and draft 5 new headline options and 3 descriptions for a Responsive Search Ad."

> "Take those headlines and post them as a new ad in ad group [ad group resource name] — keep it paused."

**Important:** every new ad this connector creates is posted as **PAUSED**. Nothing spends money or goes live until you manually review it in the Google Ads dashboard and switch it to Enabled yourself.

---

## Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| 401 error | Access token expired | Repeat Part 1, Step 4 to get a new one |
| 403 error | Developer token not approved for this account, or wrong customer ID | Double-check your Customer ID (10 digits, no dashes); confirm token status in API Center |
| 400 error on posting an ad | Headline over 30 characters, description over 90, or too few headlines/descriptions | RSAs need 3–15 headlines and 2–4 descriptions, each within the character limit |
| Claude can't find the connector | Custom connector not saved correctly | Re-check Settings → Connectors and re-upload the JSON file |

---

## Notes

- This toolkit only works with a Google Ads account you own or manage — you're authenticating as yourself, not through us.
- We don't collect, store, or see your tokens, account data, or ad copy — everything runs directly between your Claude account and Google's API.
- Questions or issues: [your support email/contact here]
