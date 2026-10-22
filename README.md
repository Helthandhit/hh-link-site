# app.healthandhit.com - HealthHIT one-tap plan links (static, GitHub Pages ready)

- `p/index.html` - opened by `https://app.healthandhit.com/p#<payload>` when the App Link isn't verified (or the app isn't installed).
  Reads the plan from the `#` (never sent to a server), tries `intent://plan?d=...;scheme=healthandhit;package=com.healthandhit.healthhit`
  with the Play Store as fallback, and shows "Open in HealthHIT" + Play buttons. No network calls.
- `j/index.html` - (HealthAdmin 1.3.8) opened by `https://app.healthandhit.com/j#<payload>` - the one-tap "add this client" link in a
  HealthHIT sign-up - when HealthAdmin's App Link isn't verified. The payload is the encrypted sign-up (after the `#`, never sent
  to a server); it hands over via `intent://join?d=...;scheme=hhcoach;package=com.healthapp.admin`. No network calls.
- `c/index.html` - the same fallback page for HealthHIT 1.2.47+ weekly check-in links (`https://app.healthandhit.com/c#...`),
  handing over via `intent://checkin?d=...;scheme=hhcoach;package=com.healthapp.admin`. HealthAdmin 1.3.9+ opens `/c` itself.
  No assetlinks change needed (the HealthAdmin statement covers the whole domain). No network calls.
- `f/index.html` - (HealthAdmin 1.4.0 / HealthHIT 1.2.48) opened by `https://app.healthandhit.com/f#<payload>` - the coach's
  "Reviewed + note" feedback link - when HealthHIT's App Link isn't verified or the app isn't installed. The payload (after the `#`,
  never sent to a server) is signed by HealthAdmin like a plan (not encrypted - it's the coach's own note to the client). It hands over
  via `intent://feedback?d=...;scheme=healthandhit;package=com.healthandhit.healthhit` with Google Play as fallback.
  HealthHIT 1.2.48+ opens `/f` itself. No assetlinks change needed (the HealthHIT statement covers the whole domain). No network calls.
- `.well-known/assetlinks.json` - lets Android open HealthHIT straight from the link (App Link). **Replace the placeholder with the
  Play Console "App signing key certificate" SHA-256** (Setup > App integrity). The second entry is the upload key (my-release-key.jks).
  The second statement (HealthAdmin 1.3.8) lets HealthAdmin (`com.healthapp.admin`, sideloaded, signed with the same key) open `/j` links.
- `CNAME` - `app.healthandhit.com`. `.nojekyll` - required, otherwise GitHub Pages hides `.well-known`.

## Go live
1. New public GitHub repo (e.g. `hh-link-site`), upload these files (include the dot-files), Settings > Pages > Deploy from branch `main` / root.
2. In the DNS for healthandhit.com (WordPress.com > Domains > DNS records) add `CNAME  app  ->  <your-github-username>.github.io`.
3. GitHub Pages > Custom domain `app.healthandhit.com`, tick "Enforce HTTPS" once the certificate is issued.
4. Check `https://app.healthandhit.com/.well-known/assetlinks.json` loads, then test with
   `https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://app.healthandhit.com&relation=delegate_permission/common.handle_all_urls`
5. On the phone (HealthHIT 1.2.41+): Settings > Apps > HealthHIT > Open by default should show app.healthandhit.com as verified.
   Android re-checks at install, so reinstall/update HealthHIT after the site is live.
