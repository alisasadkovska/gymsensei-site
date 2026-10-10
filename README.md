# GymSensei site — support, privacy and landing page

Three static pages, no build step, styled to match the app.

```
index.html     landing page   → Marketing URL (optional)
support.html   FAQ + contact  → Support URL (required)
privacy.html   privacy policy → Privacy Policy URL (required)
```

## Publish on GitHub Pages — about 5 minutes

```bash
cd ~/Documents
mkdir gymsensei-site && cd gymsensei-site
# copy the three .html files in here first
git init
git add -A
git commit -m "Site: landing, support and privacy pages"
gh repo create gymsensei-site --public --source=. --push
```

Then: repo → **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`**.
Live within a minute or two at:

```
https://alisasadkovska.github.io/gymsensei-site/
https://alisasadkovska.github.io/gymsensei-site/support.html
https://alisasadkovska.github.io/gymsensei-site/privacy.html
```

Those URLs are perfectly acceptable to App Store Review. You can point a custom
domain at the same repo later and only the hostname changes.

## Custom domain, when you buy gymsensei.app

Cloudflare Registrar sells `.app` at cost, around $12/year, and `.app` is on the
HSTS preload list so HTTPS is automatic.

1. Repo → Settings → Pages → Custom domain → `gymsensei.app`
2. At your DNS provider, add the four GitHub Pages A records for the apex
   (GitHub shows them on that settings page), or a CNAME for `www`
3. Tick **Enforce HTTPS** once the certificate is issued
4. Update the two URLs in App Store Connect

## Email

The pages and the app's Account screen both point at `support@gymsensei.app`,
which does not exist yet. Two options:

- **Cloudflare Email Routing** — free, forwards `support@gymsensei.app` to your
  Gmail. Needs the domain on Cloudflare DNS.
- **Change the address** — if you'd rather not wait, replace
  `support@gymsensei.app` in all three pages *and* in `AccountView.swift` with
  an address you already control.

A reviewer may well email the support address, so make sure it reaches you
before you submit.

## Fill in before publishing

`privacy.html` has two placeholders near the top:

```
[YOUR NAME OR COMPANY]
[YOUR COUNTRY]
```

Both are required — a privacy policy with no identifiable publisher is a
rejection risk, and in the EU it's a legal requirement.

## About the privacy policy

I wrote it against what the app actually does, as built in this session:
Firebase Auth and Firestore, Cloud Functions, OpenRouter → Google Gemini for
image analysis, Cloudflare R2 for media, StoreKit for payments, and no
analytics or ad SDKs.

**Check these claims still hold before you publish**, because the policy is a
legal commitment and the app has changed a lot:

- No analytics SDK has been added (the policy states you collect none)
- Scan and physique photos are still discarded rather than stored anywhere
- Firebase Analytics is not enabled in the Firebase console — it often is by
  default, and if it is, the "no analytics" claim is wrong and the App Store
  privacy labels need a Usage Data entry too

I'm not a lawyer, and this is not legal advice. It's an accurate description of
your app's data flows written in plain language, which is what the policy needs
to be — but if you later add analytics, ads, or a web backend of your own, it
needs updating to match.
