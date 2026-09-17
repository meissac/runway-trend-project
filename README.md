# Runway Style & Color Trend Intelligence

A dual-purpose portfolio project demonstrating both applied AI/data science and business trend-analysis skills. Built end-to-end on a stratified sample of the [Vogue Runway Images Dataset](https://archive.org/details/VogueRunway_dataset) (1.28M images, 1988–2024): image embedding, unsupervised clustering, color extraction, SQL, exploratory analysis, a business taxonomy, an interactive Power BI dashboard, and an Excel trend tracker.

**Two decades of runway data (2005–2023), one central question:** which style and color trends are genuinely rising or falling — and which apparent "designer reinvention" is actually just the industry moving together?

## Key Findings

| # | Finding | Key caveat |
|---|---|---|
| 1 | **Eclectic prints are the clearest rising trend** (+13.6pp share, 2005–2023) | Survives excluding 2021 (a pandemic-disrupted season that inflated but didn't create the trend) |
| 2 | **Classic tailoring and monochrome looks are declining** — and carry the two strongest verified black color signatures in the dataset | Real garment-color shift, not a labeling artifact |
| 3 | **Black is fashion's universal neutral** — 43.2% of all looks dataset-wide, 42% even within the most colorful cluster | Independently spot-checked (5/6 sampled looks genuine) |
| 4 | **Most "designer reinvention" is the industry moving together** — only 5 of 21 shifting designers move independently of the overall trend | Christian Dior, Moschino, Marni, Undercover, and Versace are the exceptions |

Full write-up: [`docs/insights.md`](docs/insights.md)

## What's in this repo

| Path | Description |
|---|---|
| [`notebooks/runway_trend_notebook.ipynb`](notebooks/runway_trend_notebook.ipynb) | The full technical pipeline: sampling → CLIP embeddings → clustering → color extraction → SQL → exploratory analysis → business taxonomy. Fully executed with real outputs. |
| [`notebooks/runway_trend_notebook.pdf`](notebooks/runway_trend_notebook.pdf) | Static PDF export of the same notebook, for quick reading without a Jupyter environment. |
| [`data/`](data/) | Derived data only (metadata, numeric embeddings, and the SQLite database) — see [Data & Copyright](#data--copyright) below. |
| [`dashboard/`](dashboard/) | Power BI dashboard (`.pbix`) and screenshots of its 4 pages. |
| [`excel/`](excel/) | Excel trend tracker workbook, self-built with PivotTables and a Slicer. |
| [`docs/insights.md`](docs/insights.md) | The 4 key findings, written once technically and once for a business audience. |
| [`docs/taxonomy.csv`](docs/taxonomy.csv) | The business-facing trend taxonomy (lifecycle stage, business name, momentum per cluster). |
| [`docs/sampling_methodology.md`](docs/sampling_methodology.md) | Detailed sampling methodology and rationale. |
| [`docs/DATA_SOURCE_AND_COPYRIGHT.md`](docs/DATA_SOURCE_AND_COPYRIGHT.md) | What's included in this repo, what isn't, and why. |

**Not included yet:** a client-facing PowerPoint trend report is in progress and will be added separately.

## Methodology Highlights

This project's documentation is deliberately honest about what went wrong and how it was caught, not just what the final numbers say:

- **A photography-era artifact** in the clustering was found, isolated, and excluded — a cluster that was 95%+ pre-2010 images with zero garment coherence, an artifact of Vogue's older photography style rather than a real fashion category.
- **A reproducibility bug** was caught mid-project: a dependency reinstall silently shifted which numeric ID a cluster was assigned, even with a fixed random seed. Fixed by identifying clusters by content, never by hardcoded index.
- **A raw-count vs. share-based metric mistake** was caught and fixed twice — once in the original analysis, once again when it reappeared in the Power BI dashboard build.
- **A single-year statistical spike** (2021, a pandemic-disrupted runway season) was stress-tested rather than taken at face value — the top finding was re-validated with that year excluded before being reported.
- **A stage-lighting contamination issue** was found via visual spot-check, ruling out ~50% of the "orange" color-signature results as unreliable rather than reporting them uncritically.

Every one of these is documented in place in the notebook, not smoothed over.

## Data & Copyright

No original Vogue Runway photographs are included in this repository. See [`docs/DATA_SOURCE_AND_COPYRIGHT.md`](docs/DATA_SOURCE_AND_COPYRIGHT.md) for full details on what's included, what's deliberately excluded, and how to reproduce the pipeline from the original dataset if you have your own access to it.

## Tech Stack

**Technical pipeline:** Python, `open_clip` (CLIP ViT-B/32), UMAP, scikit-learn (K-means), `rembg` (foreground segmentation), SQLite
**Analysis & business layer:** pandas, matplotlib, SQL
**Deliverables:** Power BI, Excel

## Reproducing This Project

See the "Environment Setup" section at the top of the notebook for the lightweight dependency list and required data files. The heaviest steps (CLIP embedding, foreground-segmented color extraction) are documented as reference implementations rather than re-executed on every run — see the notebook's own notes on why.
