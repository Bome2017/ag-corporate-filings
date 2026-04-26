# Corporate A/G Research Extension

A public SEC filings study of structural distress and corporate deterioration regimes.

**Live site:** https://bome2017.github.io/ag-corporate-filings/

## Status

This repository hosts the public-facing research portal for the **Corporate A/G Research Extension**.

Final classification:

> **LOCKED / research-grade extension / not production / weaker than banking / worth preserving**

This is a static evidence portal. It is **not** a live dashboard, not a bankruptcy prediction product, not investment advice, not a credit rating, and not a replacement for credit analysis.

## What this is

The Corporate A/G Research Extension applies the A/G framework to public SEC company filings. It studies whether corporate deterioration can be separated into interpretable regimes using public data:

- **Structural distress** — broad financial-ground weakness.
- **Joint-collapse severity** — cases where structural ground and operating appearance are weak together.
- **Divergence / masking** — cases where operating appearance remains comparatively strong while structural ground is weak.

The public site summarizes the locked research findings, validation chain, limitations, and evidence documents.

## Main finding

The full-population signal survives with attenuation.

In the locked Phase 18 full-population test, the strongest A/G composite was **structural distress**, but two simple financial primitives — operating cash flow/assets and interest coverage — slightly outperformed the composite on AUROC.

The corporate extension is therefore best understood as:

> a research-grade structural interpretation layer, not a maximum-discrimination prediction tool.

## Key Phase 18 validation summary

| Geometry | Internal metric | Strict AUROC | Anchored AUROC | Role |
|---|---|---:|---:|---|
| Structural distress | `structural_distress_lam0_0` | 0.711 | 0.683 | Primary A/G composite screen |
| Joint-collapse severity | `co_collapse_lam0_0` | 0.677 | 0.642 | Near-petition / joint-deterioration geometry |
| Divergence / masking | `neg_R_lam0_0` | 0.489 | 0.510 | Not a broad Chapter 11 screen; retained for divergence regime |

Phase 18 full-population panel:

| Item | Count |
|---|---:|
| Admitted CIKs | 3,412 |
| Panel rows | 38,748 |
| Post-gate rows | 36,041 |
| Test rows | 19,410 |
| Strict/direct positives | 231 |
| All-anchored positives | 354 |

## What this is not

This repository does **not** provide:

- a bankruptcy prediction product;
- investment advice;
- a credit rating;
- a production risk model;
- a company watchlist;
- current company risk rankings;
- per-company distress scores;
- a replacement for credit analysis;
- a tool equivalent to the project’s banking dashboard.

## Why no company rankings are published

The public site deliberately does not distribute current company-level rankings, distress scores, or watchlists.

Reason:

> Publishing per-company distress scores risks misuse as a bankruptcy watchlist, which is outside the validated claim of this research extension.

The public artifact is limited to methodology, validation summaries, limitations, and evidence notes.

## Data sources

The underlying research uses public SEC EDGAR data, including:

- SEC ticker master;
- submissions metadata;
- companyfacts XBRL data;
- Item 1.03 / Chapter 11 / receivership filing anchors.

Raw SEC JSON files and full scored panels are **not** distributed from this repository.

## Repository structure

```text
.
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
└── docs/
    ├── index.html
    ├── final-freeze.html
    ├── phase18-results.html
    ├── phase19-decision.html
    ├── baseline-comparison.html
    ├── sector-analysis.html
    ├── calibration-policy.html
    ├── anchor-refresh.html
    └── divergence-anchor.html
