# MIS Solutions Website Refresh

Standalone redesign workspace for MIS Solutions, implementing a customer-facing brochure site with a shared visual system and deeper navigation.

## Repo location

`mis-modern-systems-interface/`

## Files

- `index.html` contains the homepage.
- `services.html` contains the service overview and advisory detail.
- `about.html` contains the company story and positioning.
- `contact.html` contains the contact path and engagement prompts.
- `styles.css` contains the shared visual system and responsive layout.
- `assets/mis-logo.png` packages the official MIS logo lockup used in the header.

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
