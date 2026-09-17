# Sampling Methodology — Runway Style & Color Trend Intelligence

## Source
Vogue Runway Images Dataset (Internet Archive, `VogueRunway_dataset`), scraped 2023-03-31.
1,281,633 images, 1988–2024, across 129 WebDataset shards (`vogue_runway_000.tar`–`_128.tar`, ~6.7GB each).
Metadata: `VogueRunway.parquet` (94.5MB, downloaded and inspected in full — not sampled — for this step).

## Key findings from full-metadata inspection
- **Null rates**: `city` 97.4% null (unusable), `category` 14.6% null, `tags` 4.1% null. `designer`, `season`,
  `year`, `section` fully populated.
- **Year distribution is heavily skewed toward recent seasons**: 1988–1999 combined = ~22.4k images (1.7% of
  the dataset); 2016 alone = 143.6k images. This reflects Vogue.com's actual digitization history, not a
  sampling artifact.
- **Shards are near-uniformly mixed by year**: measured "dominant year share" per shard averaged 0.112
  (std 0.0028) across all 129 shards — i.e., no shard is chronologically clumped. This means any small set
  of shards is a statistically representative cross-section of the whole dataset's year distribution, so
  full-dataset temporal coverage does not require downloading most/all shards.
- **`section` field**: 71.7% of images are tagged `Collection` (full runway look); remainder are `Details`,
  `Beauty`, `Front Row`, `Atmosphere` (close-ups/backstage/audience — excluded as noise for style/color
  analysis of full looks).
- **Designers**: 1,788 unique designers, top houses (Chanel, Valentino, Dior) each ~1.5–1.7% of the dataset —
  no designer is overwhelming enough to need special downweighting.

## Sampling decision
Chose **proportional (natural-skew) sampling** over deliberately oversampling the pre-2000 era. Rationale:
pre-2000 images are sparse (1.7% of data) *and* spread evenly across every shard rather than concentrated,
so recovering a meaningful pre-2000 subsample would require downloading ~8–10x more shards for a small,
still-statistically-thin payoff. The core business question (season-over-season trend momentum) is inherently
a recent-data question; pre-2000 content is retained as proportional context, not force-balanced.

## Procedure
1. Selected 4 shards spread across the ID range for structural diversity hedge: **0, 32, 64, 96**
   (out of 129 total).
2. Filtered candidate pool to `section == 'Collection'` → 28,640 candidate images across 1,591 designers.
3. Drew a stratified-by-year sample proportional to each year's share of the candidate pool, target
   **n = 4,000**, `random_state = 42`.
4. Streamed each of the 4 shard `.tar` files directly from Internet Archive (no full 6.7GB tar persisted to
   disk); matched tar members against the manifest's `filename` column; extracted only the 4,000 wanted files.
5. **Result**: 4,000/4,000 files matched and extracted (100% match rate on all 4 shards — no naming-convention
   issues). Integrity check: 0 corrupted/unreadable files. Visual spot-check (n=12, random) confirmed
   labels (designer/season) plausibly match image content, no decode artifacts.

## Known limitations
- Pre-2000 seasons are represented by single-digit-to-low-double-digit image counts (e.g., 1990: n=3 in the
  final sample) — not statistically usable for standalone pre-2000 trend claims. Any pre-2000 findings in
  later phases should be flagged as illustrative/anecdotal only, not trend-supported.
- Sample drawn from only 4 of 129 shards (~3.1% of the full dataset). Designer coverage: candidate pool had
  1,591 of 1,788 total designers represented (89%); very-low-count designers (<15 images dataset-wide) may
  be under- or un-represented in the final 4,000.
- No deduplication check across near-identical consecutive runway shots (e.g., same look photographed from
  slightly different angles) — worth a light check in Phase 3 if style clusters look suspiciously tight.

## Files
- `VogueRunway.parquet` — full metadata (all 1.28M rows)
- `sample_manifest.csv` — the 4,000-row selected sample with full metadata
- `sample_images/` — the 4,000 extracted `.jpg` files
- `spot_check_grid.png` — visual QC grid
