# NA Hull Generator

Generate NavalArt-compatible hulls from OBJ models or procedural parameters.

> Current focus: import an OBJ hull model, choose axis mapping and target length, preview it in the browser, generate a first-pass NavalArt `.na` hull made from adjustable hull blocks, and export the result.

## MVP v0.1 Scope

This first version is intentionally small and practical:

- Import `.obj` files
- Select which OBJ axis is used as ship length, width, and height
- Flip each axis if the model direction is wrong
- Uniformly scale the model by target ship length
- Preview the transformed OBJ point cloud / wire hull in a browser
- Generate a basic NavalArt hull using `part id="0"` blocks
- Preview generated hull blocks
- Export a minimal `.na` file
- Leave hooks for later nation presets, ship-type presets, and optional railings

## Why OBJ is approximated instead of directly embedded

The sample `.na` files analyzed so far store hull geometry as NavalArt parts, especially adjustable hull parts using `part id="0"`. The file format does not appear to embed arbitrary external OBJ mesh data directly. Therefore, this tool converts an OBJ hull into many NavalArt hull blocks that approximate the shape.

## Install

Requires Python 3.10+.

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -e .
```

Or on Windows, run:

```bat
start.bat
```

Then open:

```text
http://127.0.0.1:7860
```

## Current Pipeline

```text
OBJ file
→ pure Python OBJ parser
→ axis mapping
→ direction flipping
→ uniform scaling by target length
→ station sampling along the chosen length axis
→ vertical layer splitting
→ basic `part id="0"` hull block generation
→ browser preview
→ `.na` export
```

## Development Roadmap

### v0.1

- [x] Repository skeleton
- [x] OBJ loader
- [x] Axis mapping settings
- [x] Target length scaling
- [x] Browser preview page
- [x] Basic hull block generation
- [x] `.na` export

### v0.2

- [ ] Better mesh slicing by triangle intersection instead of point sampling
- [ ] Smoother station curves
- [ ] Better `frontSpread` / `backSpread` fitting
- [ ] Waterline, red bottom, and gray upper hull color split
- [ ] More accurate bow/stern handling
- [ ] Config JSON import/export

### Later

- [ ] Nation presets: US, Soviet, Japanese, German, British, etc.
- [ ] Ship type presets: destroyer, cruiser, battleship, carrier, battlecruiser
- [ ] Optional railings
- [ ] Error heatmap between OBJ and generated NA hull
- [ ] Packaged Windows release

## Notes from sample `.na` files

The uploaded sample files showed that hulls are mostly made from `part id="0"` blocks. Important fields include:

- `length`
- `height`
- `frontWidth`
- `backWidth`
- `frontSpread`
- `backSpread`
- `upCurve`
- `downCurve`
- `heightScale`
- `heightOffset`
- `position`
- `rotation`
- `scale`
- `color`
- `armor`

The internal coordinate convention used by this project is:

```text
NA X = width / left-right
NA Y = height / up-down
NA Z = length / bow-stern
```

## Status

Experimental. Exported hulls should be tested in NavalArt frequently.
