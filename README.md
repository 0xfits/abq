# abq

Hosts two things side by side:

- **`/`** — the Abbey Quant public site: a five-page static brochure (Home, About, Approach, Technology, Contact) for Abbey Quant, a quantitative market-making firm.
- **`/abbey/*`** — one path-preserving reverse-proxy rewrite to the abbey platform app, per the integration contract in `abbey.spec.md` §9.2 (prefix not stripped; a single rule covers static, SPA, API and cookie paths; no shared auth).

Vercel resolves this without conflict: the static files win for everything at the root, and `/abbey/*` falls through to the rewrite.

## Deploy shape

Zero build, zero framework, zero JavaScript, zero external requests. Deployed on Vercel with Framework = Other, no build command, output directory = repo root. All pages are plain HTML sharing one `styles.css` and system fonts only; the logo is the only image asset and is served same-origin.

Local preview: `python3 -m http.server 8014 --bind 127.0.0.1`.

## Public surface discipline

Everything git-tracked is the public surface, so the file list is a whitelist of exactly ten entries:

```
index.html
about/index.html
approach/index.html
technology/index.html
contact/index.html
styles.css
assets/abbey-quant-logo.png
vercel.json
README.md
.gitignore
```

Anything outside that list does not belong in the repo — no scratch files, notes, specs or build artefacts. The logo in `assets/` is a byte-for-byte copy of the brand master (kept outside this repo); its sha256 is `d1b3d348272c17c6cce5588e50de40caf8fe8a9597eecfe6d656d266208ce4dc`, which any deployment check should verify. Changes to `vercel.json` must pass `@vercel/routing-utils` validation before they land.
