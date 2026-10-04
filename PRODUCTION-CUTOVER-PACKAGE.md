# misfirm.com production cutover record

Updated 2026-10-03 after the Chief of Staff completed the production DNS change. This document records the observed configuration and approved operating decisions. Jordan Hale did not change DNS or production hosting.

## Decision

**CUTOVER COMPLETE — GO for the explicitly approved apex-only configuration, with accepted rollback and `www` risks.**

The earlier release candidate in this repository and its original NO-GO gate were superseded when the Chief of Staff selected the outside firm's one-page GitHub Pages site and performed the Route 53 update. The production source identified by the Chief of Staff is <https://numerate64.github.io/misfirm.com/>. This repository's approved preview remains a historical release candidate and is not the source currently served at `misfirm.com`.

## As-built production configuration

- Canonical production URL: <https://misfirm.com>
- Scope: apex only; the Chief of Staff explicitly decided that `www.misfirm.com` is unsupported.
- DNS: Route 53 non-alias A record, Simple routing, TTL 300, no health check shown.
- A values: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, and `185.199.111.153`.
- Hosting observed: GitHub Pages (`server: GitHub.com`).
- Site architecture observed: one-page site with `#services`, `#about`, `#approach`, and `#contact` anchors; separate `/services`, `/about`, and `/contact` routes are not part of this production build.

The Route 53 values above were confirmed from the Chief of Staff's attached console screenshot and matched public DNS. Repository administration for `numerate64/mis-modern-systems-interface` was previously confirmed, but it does not establish administration of the separately selected production source.

## Verification

Verified read-only on 2026-10-03 EDT:

- `https://misfirm.com` returns HTTP 200 over HTTPS from GitHub Pages.
- Public apex DNS returns all four GitHub Pages A records.
- `www.misfirm.com` has no public A, AAAA, or CNAME response, consistent with the approved apex-only decision.
- The Route 53 screenshot shows Simple routing, TTL 300, and no health check for the apex A record.
- Production DNS and hosting were not changed during preparation or this verification.

## Rollback posture

The Chief of Staff explicitly decided that no rollback window is required and no former distribution must be kept warm. If rollback is later required, the stated fallback is to re-enable the former CloudFront endpoint to its S3 bucket and restore its Route 53 target.

That fallback was not independently tested, and the former target values were not preserved in this package. It is therefore a recovery concept, not a ready or time-bounded rollback procedure. An authorized Route 53/CloudFront operator must identify and validate the former distribution before relying on it.

## Accepted and residual risks

- `www.misfirm.com` does not resolve by explicit decision; visitors using `www` will fail to reach the site.
- There is no maintained rollback window or tested rollback procedure.
- The selected production source is outside this repository, so the repository access verified for the historical release candidate does not prove access to update or recover production.
- The production footer links to `linkedin.com/company/misfirm/`, while the company-designated canonical LinkedIn page is `linkedin.com/company/mis-solutions-llc/`. This should be corrected in the production source by its owner.
- The selected production site is a one-page build; direct multi-page paths from the superseded preview return HTTP 404 and should not be advertised.

## Historical release candidate

- Repository: <https://github.com/numerate64/mis-modern-systems-interface>
- Original owner-approved revision: [`38631722bb649fd4df6ca0506d455b5121d24fa4`](https://github.com/numerate64/mis-modern-systems-interface/commit/38631722bb649fd4df6ca0506d455b5121d24fa4)
- Final preview revision before the outside-site selection: [`45b01a0b7f7f0d950afdf0091dfe67378fa6d95e`](https://github.com/numerate64/mis-modern-systems-interface/commit/45b01a0b7f7f0d950afdf0091dfe67378fa6d95e)
- Preview: <https://numerate64.github.io/mis-modern-systems-interface/>

No further cutover action is required by this package. Any correction to `www`, the LinkedIn destination, production-source access, or rollback readiness should be handled as separately authorized follow-up work.
