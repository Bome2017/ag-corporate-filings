# Corporate A/G Public Portal V2 — Operator Deployment README

**Audience:** the operator (Bob), not the public.
**Status:** This file lives in `public_staging_v2/_operator/`
and **must NOT be uploaded** to the public GitHub Pages site.
The directory name starts with `_` to make that intent
visually obvious.

If you accidentally upload this file, the public will see your
internal deployment notes. Don&rsquo;t.

---

## 1. What this folder is

A self-contained static site that ships the Corporate A/G
Research Extension as a public evidence portal. It is built
entirely from the locked Phase-19 corporate source documents
in the project repo. Tree:

```
public_staging_v2/
├── index.html
├── methodology.html
├── validation.html
├── evidence.html
├── limitations.html
├── commercial-use.html
├── assets/
│   ├── style.css
│   ├── corporate_summary.json
│   ├── corporate_validation_phase18.json
│   ├── corporate_phase19_summary.json
│   └── corporate_document_links.json
├── docs/
│   ├── index.html
│   ├── final-freeze.html
│   ├── phase18-results.html
│   ├── phase19-decision.html
│   ├── baseline-comparison.html
│   ├── sector-analysis.html
│   ├── calibration-policy.html
│   ├── anchor-refresh.html
│   └── divergence-anchor.html
└── _operator/
    └── README_DEPLOY.md   (this file — DO NOT UPLOAD)
```

No external JavaScript or CSS dependencies. Pages render from
`file://` paths and from GitHub Pages without modification.

## 2. What to upload

Upload **the contents of `public_staging_v2/`** EXCLUDING
`_operator/`. The clean way is:

```bash
# from the parent of public_staging_v2/:
rsync -a --exclude '_operator/' public_staging_v2/ <upload-target>/
```

or, if you prefer `cp`:

```bash
mkdir -p <upload-target>
cd public_staging_v2
cp -R index.html methodology.html validation.html \
       evidence.html limitations.html commercial-use.html \
       assets docs <upload-target>/
```

## 3. What NOT to upload

- `_operator/` — operator-only deployment notes (this file).
- Raw SEC `submissions/CIK*.json` files (gigabytes; reproducible from public SEC URLs).
- Raw SEC `companyfacts/CIK*.json` files (gigabytes).
- Raw `8k_bodies/` HTML.
- The full Phase-18 firm-year panel CSV (~30 MB; per-CIK data; could be misread as a public ranking).
- Per-firm-year scores and geometries CSVs.
- Per-CIK fetch queue / log files.
- Any current company ranking / top-N list / watchlist.
- `.DS_Store`, `__MACOSX/`, `.git/` if you are zipping.

## 4. Deployment Option A — separate repo (recommended)

```bash
# Create a new GitHub repo, e.g. ag-corporate-research-portal.
# Locally, init it and copy the public contents (excluding _operator/) into root.

cd ~/ag-corporate-research-portal      # the new repo root, after `git init`
rsync -a --exclude '_operator/' \
      ~/ag_scoreboard/Domain_Extensions/Corporate_Filings/public_staging_v2/ \
      ./

git add .
git commit -m "Initial corporate A/G research portal V2 — Phase 19 freeze"
git push origin main

# Then on github.com:
#   Settings → Pages → Source: Deploy from a branch → Branch: main, /(root)
```

After a minute or two, the portal is live at
`https://<user-or-org>.github.io/ag-corporate-research-portal/`.

## 5. Deployment Option B — `/docs` folder in an existing repo

```bash
cd ~/some-existing-public-repo
mkdir -p docs
rsync -a --exclude '_operator/' \
      ~/ag_scoreboard/Domain_Extensions/Corporate_Filings/public_staging_v2/ \
      docs/

git add docs
git commit -m "Add corporate A/G research portal V2 — Phase 19 freeze"
git push

# Then on github.com:
#   Settings → Pages → Source: Deploy from a branch → Branch: main, /docs
```

Live at `https://<user-or-org>.github.io/<existing-repo>/`.

## 6. Local testing

Open any HTML page directly in a browser:

```bash
open ~/ag_scoreboard/Domain_Extensions/Corporate_Filings/public_staging_v2/index.html
```

For a quick local web server (optional):
```bash
cd ~/ag_scoreboard/Domain_Extensions/Corporate_Filings/public_staging_v2
python3 -m http.server 8000
# then visit http://localhost:8000/
```

All internal links use relative paths and work both from
`file://` and from GitHub Pages.

## 7. Final checks before publishing

Run each from `public_staging_v2/`:

```bash
# 7.1 No __MACOSX folders.
find . -type d -name '__MACOSX' && echo "OK: none" || true

# 7.2 No [contact email] placeholder.
grep -RIE '\[contact email\]' . --include='*.html' --include='*.md' --include='*.json' \
   && echo "FAIL: placeholder remaining" \
   || echo "OK: no placeholder"

# 7.3 No /Users/bsm or other absolute paths in public files (everything except _operator/).
grep -RIE '/Users/|~/ag_scoreboard|/Library/|/sessions/' \
   --exclude-dir=_operator . \
   --include='*.html' --include='*.md' --include='*.json' --include='*.css' \
   && echo "FAIL: path leak" \
   || echo "OK: no path leaks in public surface"

# 7.4 No CSV files.
find . -name '*.csv' && echo "FAIL: CSV present" || echo "OK: no CSVs"

# 7.5 No files larger than 1 MB (other than this README, which is small).
find . -type f -size +1M

# 7.6 No raw SEC JSONs.
find . -name 'CIK*.json' && echo "FAIL: raw SEC JSON present" || echo "OK"

# 7.7 No .DS_Store.
find . -name '.DS_Store' && echo "FAIL: macOS metadata present" || echo "OK"
```

If any of 7.2 / 7.3 / 7.4 / 7.6 / 7.7 returns a hit, fix it
before pushing.

Visual checks:

- [ ] Open `index.html` in a browser. Click every nav link.
  Confirm all six pages render and `docs/index.html` opens.
- [ ] Confirm the contact in `commercial-use.html` is
  `ag.bank.scoreboard@gmail.com` (or the project&rsquo;s
  preferred public email if you decide otherwise).
- [ ] Spot-check the validation table on `validation.html`
  matches the source numbers in
  `Corporate_Filings/CORPORATE_PHASE18_FULL_POPULATION_RESULTS.md`.

## 8. How to update later (Phase 20+)

Only do this if a Phase 20 has produced new locked
documents. Phase 19 is the canonical state until a documented
new phase exists.

```bash
# 1. Re-run the wording / file audit on any updated docs.
# 2. Update the relevant HTML page(s) in public_staging_v2/.
# 3. Refresh assets/corporate_summary.json,
#    corporate_validation_phase18.json (or successor),
#    corporate_phase19_summary.json (or successor),
#    and corporate_document_links.json with the new numbers.
# 4. Update each HTML page's "freeze date" line and status banners.
# 5. Re-run the audit checks in §7.
# 6. Push.
```

If a Phase 20 produces evidence that changes doctrine (e.g.,
external divergence-anchor validation), prefer to add a new
`docs/phase20-*.html` page rather than overwrite the
Phase-18 / Phase-19 narrative.

## 9. Public claim discipline (for your own sanity)

The site is constrained by the locked claim discipline in
`docs/final-freeze.html`. Don&rsquo;t add anything to the
public surface that violates these:

- Allowed: "research-grade", "preliminary full-population
  screening evidence", "ranked monitoring / triage", "weaker
  than the banking branch", "not production-ready", "not a
  bankruptcy prediction product", "domain-specific A/G
  transfer".
- Forbidden: "predicts bankruptcy", "validated commercial
  bankruptcy predictor", "production model", "regulatory-grade",
  "replaces credit analysis", "investment advice", "credit
  rating", "top bankruptcy risks", "highest-risk companies",
  "likely to fail", "works as well as banking".

The wording audit in
`Corporate_Filings/CORPORATE_PUBLIC_PORTAL_V2_AUDIT.md`
records the result of a forbidden-phrase scan over this
folder.

## 10. Discipline reminder

The portal is intentionally a research evidence portal, not a
"company ranking" or "distress dashboard." If you find
yourself wanting to add a per-company table, a top-N list, or
a "current high-risk firms" section, **stop**. That is exactly
the kind of presentation the freeze documents disallow. The
operator&rsquo;s discipline on what the corporate extension
can and cannot claim is the most valuable thing about this
site.
