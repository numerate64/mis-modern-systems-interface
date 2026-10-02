# misfirm.com production cutover package

Prepared 2026-10-02 for the approved website revision. This package is operational guidance only; it does not authorize or perform a DNS change.

## Decision

**NO-GO until Route 53 hosted-zone access is confirmed and the current apex and `www` record sets are exported for rollback.**

The owner-approved website, source revision, GitHub repository administration, Pages deployment, and rollback sequence are otherwise ready. Do not begin the custom-domain or DNS steps until the remaining gate is closed.

## Approved release

- Repository: <https://github.com/numerate64/mis-modern-systems-interface>
- Approved commit: [`38631722bb649fd4df6ca0506d455b5121d24fa4`](https://github.com/numerate64/mis-modern-systems-interface/commit/38631722bb649fd4df6ca0506d455b5121d24fa4)
- Preview: <https://numerate64.github.io/mis-modern-systems-interface/>
- Production site before cutover: <https://misfirm.com>

## Verified production configuration

- `main` and `origin/main` both resolve to the approved commit.
- GitHub reports repository permissions `admin`, `maintain`, and `push` for the connected operator.
- GitHub Pages is built from `main` at `/`; the preview is live and no custom domain is currently configured (`cname: null`).
- The homepage, Services, About, and Contact preview URLs return HTTP 200.
- Existing `misfirm.com` and `www.misfirm.com` currently resolve to AWS/CloudFront addresses. They were inspected read-only and were not changed.
- No root `CNAME` file exists at the approved commit. Adding it is part of the authorized cutover, not preparation.

## Preflight gate

The release operator must check every item immediately before cutover:

- [x] Owner approval identifies the preview and approved commit above.
- [x] Connected GitHub operator has repository administration access.
- [x] Pages source is `main` and `/`, and the approved preview is healthy.
- [ ] Route 53 operator can open the authoritative `misfirm.com` hosted zone and change records.
- [ ] Export or screenshot the complete current apex and `www` record sets, preserving type, value/alias target, routing policy, health-check/evaluate-target-health settings, and TTL.
- [ ] Confirm the former production distribution remains available during the rollback window.
- [ ] Confirm `info@misfirm.com` is monitored.
- [ ] Name the cutover operator and rollback decision-maker and agree on an observation window.

## Cutover procedure

1. Save the current apex and `www` record-set export outside the repository. Do not alter MX, TXT, SPF, DKIM, DMARC, or unrelated records.
2. If supported by the current records, lower only the web-record TTLs to 300 seconds at least one existing TTL window before cutover.
3. Add a root `CNAME` file containing exactly `misfirm.com` to `main`, then wait for the Pages build to pass.
4. In **Settings → Pages**, set the custom domain to `misfirm.com`.
5. At execution time, use GitHub's current official Pages documentation for the apex record targets; do not copy hard-coded IP addresses from an old runbook.
6. Replace only the apex web records in Route 53. Set `www.misfirm.com` to CNAME `numerate64.github.io`.
7. Verify apex and `www` through at least two public resolvers. Load all four production pages over HTTP and HTTPS.
8. When GitHub reports the certificate ready, enable **Enforce HTTPS** and verify the canonical-host redirect and certificate.
9. Smoke-test navigation, favicon/logo, LinkedIn, and `mailto:info@misfirm.com` on desktop and a real mobile device.
10. After the agreed stable observation window, restore normal TTLs and record the final values and timestamps.

## Rollback decision and procedure

Rollback immediately if DNS resolution, TLS issuance, canonical redirects, page rendering, navigation, or the contact path fails during the observation window.

1. Restore the saved Route 53 apex and `www` record sets exactly, including aliases, routing policies, health-check settings, and TTLs. Do not touch email or unrelated records.
2. Verify the former production host at both apex and `www` over HTTPS through multiple public resolvers.
3. Remove or change the Pages custom domain only after DNS restoration is confirmed. Keep the repository and approved commit available for diagnosis.
4. For a code-only defect after a successful domain/TLS cutover, revert the defective launch commit and let Pages redeploy; retain the custom domain. Use DNS rollback for routing, hosting, or TLS failures.
5. Record the trigger, decision time, records restored, verification results, and follow-up owner before another attempt.

## Residual, non-blocking items

- The repository requests `Quivera Regular` but does not contain a licensed webfont, so fallback fonts render today.
- Approved Open Graph image/copy is not yet present.
- The final real-device smoke test necessarily occurs after custom-domain and TLS activation.

## Unblock owner and action

Chief of Staff: provide or assign an authorized Route 53 operator who can confirm access to the `misfirm.com` hosted zone and attach a sanitized export of the current apex and `www` web record configuration. Once that evidence exists, the cutover package changes from **NO-GO** to **GO**, subject to the preflight checklist.
