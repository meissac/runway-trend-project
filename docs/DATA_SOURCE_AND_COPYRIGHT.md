# Data Source & Copyright

## Source dataset

This project uses the [Vogue Runway Images Dataset](https://archive.org/details/VogueRunway_dataset) hosted on Internet Archive: approximately 1.28M runway images spanning 1988–2024, originally sourced from Vogue Runway's public archive. The dataset's own listing should be consulted for its terms of use.

This project uses a stratified sample of that dataset — 4,000 images sampled proportionally across shards, years, and the "Collection" (full-look) category. See [`sampling_methodology.md`](sampling_methodology.md) for the full sampling rationale.

## What is included in this repository

Everything in this repo is **derived data or original analysis output** — nothing here reproduces the original photographs:

- `data/sample_manifest.csv` — metadata only (designer, season, year, category, filename references). No image content.
- `data/clip_embeddings_final.npz` — numeric CLIP embedding vectors (512-dimensional floats per image). These are a mathematical transformation of image content for machine learning purposes, not a visual reproduction of the images themselves.
- `data/runway_trends.db` — a SQLite database containing our own clustering results, color extraction outputs (as RGB numeric values, not images), and business taxonomy. Entirely derived analytical output.
- All charts, the Power BI dashboard, and the Excel workbook — visualizations built from the numeric/tabular data above, not from the source images.

## What is deliberately excluded

- **No original Vogue Runway photographs** appear anywhere in this repository, in any notebook, document, or dashboard screenshot.
- Visual spot-checks performed during development (e.g., verifying that extracted colors matched actual garment colors) are described narratively in the project documentation rather than reproduced with the source images.
- The full extracted image sample (`sample_images.zip`) and any intermediate visual validation grids are kept outside this repository entirely.

## Reproducing the pipeline yourself

If you have your own access to the Vogue Runway Images Dataset (or an equivalent licensed image source), the notebook's reference implementation cells show the exact methodology used for:
- Streaming and sampling images from the source archive
- CLIP image embedding
- Foreground-segmented color extraction

These are included as documented reference code (not executed against images in this repo) so the methodology is fully transparent and reproducible by anyone with appropriate access to image data, without this repository itself needing to host or redistribute copyrighted photographs.
