# Metrics Improvement Notes

## Current Status

The current implementation captures core trial outcomes:

- naming onset RT and missed onset flags
- typed category labels (raw and trimmed)
- pleasure, ambiguity, and naming confidence ratings
- trial linking (`trial_uid`) and phase (`practice` vs `main`)
- basic QC flags (e.g., very fast RT, no naming response, straightlining)

This is enough for basic analyses, but not yet ideal for high-rigor reporting.

## Gaps To Address

- Missing browser/device metadata in saved rows (browser, version, screen size, mobile state).
- Limited timing decomposition after naming onset (no explicit typing RT metric; rating RTs not harmonized into named fields).
- `typed_label_empty_attempts` is likely low-information because HTML required inputs block empty submissions before `on_finish`.
- No explicit metric for duplicate/accidental onset keypress behavior.
- No explicit per-trial completion validity flag (all required stages present for a given `trial_uid`).
- No participant-level engagement flag computed from trial-level quality signals.

## Recommended Upgrades (Next Pass)

1. Add global environment metadata:
   - `browser`, `browser_version`, `screen_width`, `screen_height`, `mobile`
2. Add stage timing outputs:
   - `category_input_rt_ms`
   - named rating RT fields (`rating_pleasure_rt_ms`, `rating_ambiguity_rt_ms`, `rating_confidence_rt_ms`)
3. Add trial completeness checks:
   - `trial_complete` boolean by `trial_uid`
   - `flag_trial_invalid` for missing required stage rows
4. Improve label quality metrics:
   - `label_char_count`
   - `label_token_count`
   - optional `flag_label_low_information` rule
5. Add participant-level quality summary fields (computed post hoc):
   - `flag_participant_low_engagement`
   - threshold-based summaries (timeout rate, straightline rate, ultrafast RT rate)

## Suggested Priority

- **High**: metadata, stage RT fields, trial completeness flags
- **Medium**: label quality metrics
- **Medium**: participant-level aggregate flags

## Notes

- Keep raw typed responses unchanged for reproducibility.
- Prefer deriving exclusion decisions in analysis scripts, while storing transparent trial-level flags in raw data.
