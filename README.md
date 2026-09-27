# MIS Modern Systems Interface Homepage

Standalone redesign workspace for `MIS-11`, implementing the approved one-page MIS homepage in the selected Option C direction.

## Repo location

`mis-modern-systems-interface/`

## Files

- `index.html` contains the one-page homepage.
- `styles.css` contains the visual system and responsive layout.
- `assets/mis-logo.png` packages the official MIS logo lockup used in the header.

## Run locally

No build step is required.

From this directory, serve the files with:

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173`.

## Notes

- The page keeps the approved `mailto:info@misfirm.com` contact route.
- The layout preserves the `v2.html` section sequence: hero, services, AI note, story, and contact CTA.
- No customer logos, partner badges, certifications, or unsupported metrics were added.

## Remaining gaps

- `Quivera Regular` is referenced in the display font stack, but no licensed webfont file was added in this repo.
- This workspace does not include production hosting, CMS wiring, or deployment into the live MIS website.
