---
name: concertina-abc-annotation
description: 'Convert or generate Anglo concertina ABC under-note annotations for the AngloGlyphs font. USE THIS SKILL for legacy annotation conversion, new ABC fingering annotation, note-to-glyph mapping checks, numbering-system mismatches, or AngloGlyphs PUA selection.'
---

# Concertina ABC Annotation

Use this skill when working on ABC notation files in this repository that need Anglo concertina button annotations rendered with the AngloGlyphs font.

## Read this first

1. Read `references/workflow.md`.
2. Read `data\manifest\glyphs.json`.
3. Read the target ABC file before changing it, usually under `abc\`.
4. If needed, confirm that the repository root contains `AngloGlyphs.otf` so the selected PUA glyphs can be rendered.

## Source of truth

- Treat `data\manifest\glyphs.json` as the canonical machine-readable mapping for this public repository.
- Treat that published mapping as Wheatstone-based unless the user explicitly asks for a different fingering system.
- For ABC annotation text, use the canonical codepoints from that mapping file.
- Do **not** silently switch between numbering systems. The canonical font numbering and legacy annotation numbering may differ.

## Output format

- ABC under-note annotations use a quoted string beginning with `_`, for example `" _..."` is wrong but `"_..."` is correct.
- Each annotation should contain the underscore marker plus exactly one AngloGlyphs PUA character unless the user explicitly wants mixed text.
- Preserve surrounding ABC rhythm, bars, spacing, repeats, and headers.

## Supported workflows

### 1. Convert existing annotations

Use this when the ABC already has fingering or button annotations in another notation system.

1. Identify the source annotation system from the file or user instructions.
2. Build an explicit crosswalk from the legacy tokens to AngloGlyphs glyph choices.
3. Validate that the crosswalk preserves the intended fingering, not just pitch.
4. Replace legacy annotations with AngloGlyphs PUA annotations under the same notes.
5. If the source system is ambiguous, stop and ask instead of guessing.

Important:

- Repeated notes do **not** prove repeated fingerings.
- Do not infer a token mapping solely from pitch recurrence.
- If the user has already corrected a few notes by hand, use them as anchors but still verify the rest of the token system.

### 2. Annotate unannotated ABC

Use this when the ABC has notes but no concertina fingering.

Apply heuristics in this order:

1. **Hard constraints**: only choose buttons whose note label matches the ABC pitch spelling and octave.
2. **Accidental-row minimization**: only use the accidental row when required or when later heuristics clearly justify it.
3. **Local continuity**: prefer keeping nearby notes on nearby buttons and avoid unnecessary hopping.
4. **Bellows balance**: avoid awkward runs that force excessive uninterrupted push or pull sequences.
5. **Determinism**: if several options remain, choose consistently so repeated runs give the same result.

If a note has multiple plausible buttonings and the heuristic choice is musically significant, surface that uncertainty instead of pretending the choice is canonical.

## Validation rules

- Confirm whether the user wants:
  - canonical note-to-glyph mapping, or
  - preservation of a legacy fingering system.
- Check for numbering-system mismatches before converting.
- Check for Wheatstone versus Jeffries fingering-system mismatches before converting or generating annotations.
- Double-check that the selected codepoint matches the public mapping entry for the intended glyph.
- If the user's expectation cites a specific shape, direction, or button, prefer that evidence over an unverified inferred mapping.

## When not to proceed without clarification

Stop and ask if any of these are true:

- the source annotation system is unknown,
- multiple numbering schemes are in play,
- the file mixes more than one annotation convention,
- Wheatstone and Jeffries assumptions appear to be in conflict,
- the target should preserve historical fingering rather than choose a new one,
- the published font and `data\manifest\glyphs.json` appear to disagree.
