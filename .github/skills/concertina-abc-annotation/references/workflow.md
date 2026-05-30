# Concertina ABC annotation workflow

This repository uses AngloGlyphs Private Use Area glyphs in ABC under-note annotations to display Anglo concertina fingering.

These glyphs are published in the Unicode Private Use Area because there is no standard Unicode block for Anglo concertina button symbols.

## Core assumptions

- Row shape encodes notation row, not pitch family:
  - C-row = square
  - G-row = circle
  - accidental row = diamond
- Push and pull are distinct glyphs for the same button.
- Left-hand versus right-hand placement is not part of the glyph model.

## Canonical files

- `data\manifest\glyphs.json` is the canonical public mapping for annotation work in this repository.
- `AngloGlyphs.otf` is the published font file that should render those codepoints.
- `abc\` contains annotated and unannotated tune files that can be used as examples or targets.
- The published mapping metadata is based on the Wheatstone fingering system.

## Codepoint rule

For AngloGlyphs ABC annotation work, default to the canonical codepoints from `data\manifest\glyphs.json`.

If a future font release intentionally renumbers any block, treat that as an explicit migration rather than something to infer.

Do not silently reinterpret the published Wheatstone-based mapping as Jeffries fingering. If Jeffries handling is required, treat that as an explicit conversion or a future extension to the workflow.

## Conversion guidance

When converting from an existing annotation system:

1. Identify the source token system.
2. Derive a checked token-to-glyph crosswalk.
3. Verify the crosswalk against hand-corrected examples if available.
4. Replace annotations only after the crosswalk is coherent across the tune.

Never infer a full crosswalk from a small number of matching pitches alone.

## Heuristic guidance for new annotations

Start simple and improve only when evidence shows the defaults are weak:

1. Match pitch spelling and octave exactly.
2. Prefer non-accidental rows where possible.
3. Penalize finger hopping between adjacent buttons when a smoother option exists.
4. Penalize long one-direction bellows runs when a reasonable alternative exists.
5. Keep the output deterministic and explainable.

Heuristics are defaults, not historical truth. If the user asks to preserve a particular playing tradition or existing fingering, that takes precedence.
