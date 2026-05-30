# AngloGlyphs

![Annotated staff notation example](AnnotatedStaff.png)

AngloGlyphs is a font and example library for adding Anglo concertina button annotations to ABC notation.

This repository provides:

- `AngloGlyphs.otf`, the font containing the Anglo concertina Private Use Area glyphs
- `abc\`, a growing library of annotated tunes
- `data\manifest\glyphs.json`, the canonical public mapping from notes and buttons to AngloGlyphs codepoints
- `.github\skills\concertina-abc-annotation\`, a Copilot skill for converting or generating ABC under-note annotations

## What the font encodes

The glyphs represent Anglo concertina buttons by:

- shape: C-row = square, G-row = circle, accidental row = diamond
- direction: push and pull are separate glyphs
- button number: shown inside the shape

The notation is about the button choice, not left-hand versus right-hand placement.

## Installing the font

Install `AngloGlyphs.otf` in the normal way for your operating system.

- **Windows:** right-click the font file and choose **Install** or **Install for all users**
- **macOS:** open the font in Font Book and install it
- **Linux:** copy it into your user or system font directory and refresh the font cache if needed

If a web page or notation tool does not pick the font up immediately, a refresh or browser restart may help.

## Using AngloGlyphs in ABC

The basic ABC directive is:

```abc
%%annotationfont AngloGlyphs
```

Under-note annotations should be quoted strings beginning with `_` and containing one AngloGlyphs glyph:

```abc
%%annotationfont AngloGlyphs
K:Amaj
"_..."A "_..."B "_..."c
```

The exact PUA character to use comes from `data\manifest\glyphs.json`. The example tune in `abc\Miss McLeod's Reel.abc` shows the format in real use.

![Annotated ABC example](AnnotatedABC.png)

## Helpful places to work with ABC

This repository is useful alongside dedicated ABC sites and tune libraries, for example:

- [ABC Transcription Tools](https://michaeleskin.com/abctools/abctools.html)
- [The Session](https://thesession.org/)

A common workflow is:

1. find or edit a tune in one of those tools
2. add or refine AngloGlyphs annotations
3. save the resulting ABC here for reuse and sharing

## Using the Copilot annotation skill

This repo includes a Copilot skill at `.github\skills\concertina-abc-annotation\`.

Use it when you want help to:

- convert an existing annotation system into AngloGlyphs
- annotate an unannotated ABC tune
- check button-to-codepoint mappings
- catch numbering-system mismatches

The skill uses:

- `data\manifest\glyphs.json` as its public source of truth
- files in `abc\` as the normal input and output location

## Customizing the skill to match your preferences

Automatic annotation is useful, but it is not the same thing as a final musical decision. Different players may prefer different tradeoffs for fingering continuity, accidental-row usage, or bellows direction.

If you want the skill to reflect your own preferences:

1. edit `.github\skills\concertina-abc-annotation\SKILL.md` to change the instructions given to Copilot
2. edit `.github\skills\concertina-abc-annotation\references\workflow.md` to document your preferred heuristics or house style
3. keep `data\manifest\glyphs.json` unchanged unless you are intentionally changing the published codepoint mapping

Good candidates for customization include:

- whether to preserve a legacy fingering system
- how strongly to avoid the accidental row
- how strongly to avoid finger hopping
- how much bellows balance should influence automatic choices

## Manual review still matters

Automatically annotated tunes will often benefit from manual edits.

That is expected. The aim is to make annotation easier and more consistent, not to pretend that one automatic pass is always musically ideal.

If you are editing ABC in an app that does not let you choose the annotation font directly, it can be handy to copy and paste AngloGlyphs characters from Windows Character Map. See `Charmap.png`: set **Font** to `AngloGlyphs`, set **Group By** to `Unicode Subrange`, then choose **Private Use Characters** from the Unicode subrange list.

![Windows Character Map showing AngloGlyphs private-use characters](Charmap.png)

## Contributions

Pull requests are welcome for:

- new annotated tunes
- improvements to existing annotations
- corrections to the published mapping or documentation

If you contribute annotated tune material, please be willing to share your contribution under **CC BY 4.0**.

Please also make sure you have the right to submit the material you are contributing, especially if it is derived from an existing source or includes editorial work from another site or publication.
