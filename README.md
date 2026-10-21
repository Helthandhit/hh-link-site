# app.healthandhit.com - HealthHIT one-tap plan links (static, GitHub Pages ready)

- `p/index.html` - opened by `https://app.healthandhit.com/p#<payload>` when the App Link isn't verified (or the app isn't installed).
  Reads the plan from the `#` (never sent to a server), tries `intent://plan?d=...;scheme=healthandhit;package=com.healthandhit.healthhit`
  with the Play Store as fallback, and shows "Open in HealthHIT" + Play buttons. No network calls.
- `.well-known/assetlinks.json` - lets Android open HealthHIT straight from the link (App Link). **Replace the placeholder with the
  Play Console "App signing key certificate" SHA-256** (Setup > App integrity). The second entry is the upload key (my-release-key.jks).
- `CNAME` - `app.healthandhit.com`. `.nojekyll` - required, otherwise GitHub Pages hides `.well-known`.

## Go live
1. New public GitHub repo (e.g. `hh-link-site`), upload these files (include the dot-files), Settings > Pages > Deploy from branch `main` / root.
2. In the DNS for healthandhit.com (WordPress.com > Domains > DNS records) add `CNAME  app  ->  <your-github-username>.github.io`.
3. GitHub Pages > Custom domain `app.healthandhit.com`, tick "Enforce HTTPS" once the certificate is issued.
4. Check `https://app.healthandhit.com/.well-known/assetlinks.json` loads, then test with
   `https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://app.healthandhit.com&relation=delegate_permission/common.handle_all_urls`
5. On the phone (HealthHIT 1.2.41+): Settings > Apps > HealthHIT > Open by default should show app.healthandhit.com as verified.
   Android re-checks at install, so reinstall/update HealthHIT after the site is live.
