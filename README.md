# MIS Solutions Website Refresh

Standalone redesign workspace for MIS Solutions, implementing a customer-facing brochure site with a shared visual system and deeper navigation.

## Design direction

The concept presents MIS as an experienced, practical modernization advisor rather than a generic software vendor. Navy establishes trust and technical depth, orange highlights decisions and next actions, and the light grid-and-panel system gives the pages a modern systems-interface feel without obscuring the content. The official MIS logo lockup is used as supplied.

The responsive layout is intentionally static and dependency-free so stakeholders can review it quickly and the team can maintain or publish it without a framework migration.

## Repo location

`mis-modern-systems-interface/`

## Files

- `index.html` contains the homepage.
- `services.html` contains the service overview and advisory detail.
- `about.html` contains the company story and positioning.
- `contact.html` contains the contact path and engagement prompts.
- `styles.css` contains the shared visual system and responsive layout.
- `assets/mis-logo.png` packages the official MIS logo lockup used in the header.

## Page and component choices

- A shared sticky header keeps Home, Services, About, Contact, and the canonical LinkedIn page available throughout the site.
- The homepage leads with a two-column advisory message, decision-path rail, focus-area tags, service cards, company timeline, and a clear contact CTA.
- The Services page expands the three primary advisory areas: cloud foundations, Microsoft ecosystems, and practical AI readiness.
- The About page explains the infrastructure-to-modernization story without adding unapproved customer or partner claims.
- The Contact page keeps the low-friction `mailto:info@misfirm.com` route and does not introduce a form or new data-handling flow.
- Shared panel, card, button, typography, spacing, and responsive-navigation patterns live in `styles.css`, keeping future pages consistent.

## Run locally

No build step is required.

From this directory, serve the files with:

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173`.

## Notes

- The site keeps the approved `mailto:info@misfirm.com` contact route.
- The visual direction uses MIS navy and orange across all pages.
- No customer logos, partner badges, certifications, or unsupported metrics were added.

## Remaining gaps

- `Quivera Regular` is referenced in the display font stack, but no licensed webfont file was added in this repo.
- This workspace does not include production hosting, CMS wiring, or deployment into the live MIS website.
