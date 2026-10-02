# misfirm.com launch readiness

Prepared for the owner review gate. This runbook does not authorize or perform a production release.

## Recommendation

**NO-GO pending owner approval and access confirmation.** The static site itself is ready for review, but production must not move until the owner approves the messaging/design, the domain operator confirms Route 53 access, and the repository owner confirms GitHub Pages administration access. After those gates, the technical cutover is low risk and reversible.

## QA checklist

- [x] All four preview pages and shared CSS/logo return HTTP 200.
- [x] Primary navigation, footer links, internal calls to action, and `mailto:info@misfirm.com` use valid targets.
- [x] Each page has a unique title and description, a production canonical URL, theme color, and official SVG favicon reference.
- [x] Official supplied logo assets are present in PNG and SVG formats; the displayed PNG has meaningful alternative text.
- [x] One `h1` per page, semantic landmarks, labeled primary navigation, current-page state, visible keyboard focus, and reduced-motion handling are present.
- [x] Responsive rules collapse multi-column layouts at 980px and tighten navigation/content at 640px; mobile navigation remains horizontally scrollable rather than clipping links.
- [x] No contact form, tracking code, secrets, customer data, or new external integration is present.
- [ ] Owner approves buyer-facing copy and visual presentation.
- [ ] Domain and repository owners confirm the access dependencies below.
- [ ] Perform a final real-device/browser smoke test after the production custom domain and TLS certificate are active.

## Findings and fixes

1. **Fixed — canonical metadata absent.** Added `https://misfirm.com/` canonical URLs to the home page and explicit production URLs to each interior page.
2. **Fixed — favicon absent.** Added the official supplied SVG logo as the browser icon and set the browser theme color to official navy `#09135A`.
3. **Pass — links and assets.** All local references resolve in the repository. Preview pages, stylesheet, and logo return HTTP 200. LinkedIn returned its expected automated-request denial (`999`), so its canonical URL was verified by inspection rather than bot response.
4. **Pass with follow-up — responsive/accessibility.** The layout contains explicit desktop/tablet/mobile breakpoints and baseline semantic/focus/reduced-motion support. A final device smoke test remains required after custom-domain activation.
5. **Known limitation — font.** CSS requests `Quivera Regular`, but the repository has no licensed webfont file; browsers use the declared fallback stack. Obtain a web-licensed font asset before claiming exact Quivera rendering.
6. **Known limitation — social previews.** Open Graph/social-card metadata is not included because no approved social preview image/copy was supplied. This does not block basic launch, but it should be added after content approval.

## Production cutover (GitHub Pages + Route 53)

### Access dependencies

- GitHub write/admin access to `numerate64/mis-modern-systems-interface`, including Pages settings.
- AWS access to the Route 53 hosted zone for `misfirm.com` with permission to inspect and update records.
- Ability to inspect the current production distribution/hosting configuration so it can be restored.
- Owner approval for the reviewed commit and confirmation that `info@misfirm.com` is monitored.

Never place credentials or DNS exports in the repository.

### Pre-cutover

1. Record screenshots or an export of all current apex and `www` DNS records, including type, value/alias target, routing policy, and TTL. Current public checks show the apex resolves to AWS addresses and the authoritative nameservers are Route 53; do not assume the underlying distribution can be deleted.
2. In repository **Settings → Pages**, confirm deployment from the intended `main` branch/root and that the reviewed commit is live at the preview URL.
3. Add a root-level `CNAME` file containing exactly `misfirm.com`, commit it, and wait for the preview deployment to succeed. This is a cutover step and is intentionally not included before owner approval.
4. In GitHub Pages settings, set the custom domain to `misfirm.com`. Leave **Enforce HTTPS** off only while GitHub provisions the certificate.
5. Lower existing web-record TTLs to 300 seconds at least one prior TTL window before cutover where the current record type permits it.

### DNS change

1. In Route 53, replace only the apex web records for `misfirm.com` with GitHub Pages' currently documented apex targets. At the time of execution, copy the values from GitHub's official “Managing a custom domain for your GitHub Pages site” documentation rather than relying on a stale runbook value.
2. Create or update `www.misfirm.com` as a CNAME to `numerate64.github.io`. Do not alter MX, TXT, DKIM, SPF, DMARC, or unrelated service records.
3. Verify with multiple public resolvers that the apex and `www` resolve to GitHub Pages, then load all four production URLs over HTTP and HTTPS.
4. Once GitHub reports the certificate ready, enable **Enforce HTTPS**. Confirm one canonical host redirects consistently, no certificate warning occurs, and the page canonical tags still name `https://misfirm.com/...`.
5. Recheck navigation, logo/favicon, LinkedIn, and `mailto:info@misfirm.com` on desktop and mobile. Restore normal TTLs after at least one stable observation window.

## Rollback

1. If TLS, routing, rendering, or contact-path verification fails, restore the exact saved Route 53 apex and `www` record sets and their routing policies/TTLs. Do not modify email or unrelated records.
2. Remove or change the GitHub Pages custom domain only after DNS is restored; keep the repository and reviewed commit intact for diagnosis.
3. Confirm the former production host answers on both apex and `www`, including HTTPS, from multiple public resolvers.
4. If the defect is code-only after a successful domain cutover, revert the launch commit on `main`, allow Pages to redeploy, and retain the custom domain. Use DNS rollback only for hosting/domain/TLS failures.
5. Record the failure, timestamps, DNS values restored, and verification results for the owner before attempting a new cutover.

## Review links

- Preview: <https://numerate64.github.io/mis-modern-systems-interface/>
- Repository: <https://github.com/numerate64/mis-modern-systems-interface>
- Production (unchanged): <https://misfirm.com>
