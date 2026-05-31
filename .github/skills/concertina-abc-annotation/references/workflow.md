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

Work phrase-first rather than note-first. Do not greedily lock a button for one note until you have checked the next note, and usually the whole bar or repeated figure around it.

1. Match pitch spelling and octave exactly.
2. Strip ornaments such as `~` when selecting a button. Ornamented groups such as `~A2` still use the same button choice as the underlying repeated note unless the user explicitly wants mixed fingering.
3. Build the candidate button set for each note in the phrase, then eliminate candidates before choosing among them:
   - remove accidental-row candidates when a non-accidental candidate for the same pitch remains, unless a later constraint clearly requires the accidental row;
   - treat notes with only one valid button as fixed and use them to eliminate conflicting candidates on adjacent notes;
   - treat the accidental row and C row as adjacent outer rows for hopping checks even though the G row sits between them visually.
4. Reject or heavily penalize consecutive notes on different physical buttons that use the same finger number, including across barlines and repeat marks. Repeating the same physical button is fine; moving between two distinct `1` buttons, two distinct `2` buttons, or two distinct `3` buttons is usually a sign the fingering is wrong.
5. Prefer sequences that keep the same physical button when that button legitimately serves both notes, even if the bellows direction changes between push and pull.
6. Among the surviving candidates, prefer the option that leaves the next note with the widest conflict-free choice set. If still tied, prefer the option that avoids unnecessary row changes and then the lower button number.
7. Penalize long one-direction bellows runs when a reasonable alternative exists.
8. Run a post-pass playability audit over the finished phrase and rewrite any failing span instead of leaving a known bad transition in place. Audit at least for:
   - distinct same-finger transitions across adjacent notes;
   - same-finger transitions hidden by barlines or repeat marks;
   - unnecessary accidental-row use;
   - ornaments that changed button choice without changing pitch.
9. Keep the output deterministic and explainable.

Heuristics are defaults, not historical truth. If the user asks to preserve a particular playing tradition or existing fingering, that takes precedence.
