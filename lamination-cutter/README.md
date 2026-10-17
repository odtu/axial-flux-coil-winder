# Lamination Cutter

**Fibre-laser SVGs for the teeth of a yokeless (YASA) axial-flux stator, with an optional 3D-printed frame to hold them.**

Each tooth is built from flat electrical-steel sheets stacked outward in the radial direction. Every sheet is then a plain rectangle and the tooth comes out as a trapezoidal prism, so a fibre laser cuts it from ordinary 0.2–0.35 mm strip with no forming or bending.

![Lamination Cutter](docs/images/overview.png)

A single self-contained HTML file. No build step, no dependencies, no server.

**[▶ Open it in your browser](https://odtu.github.io/axial-flux-coil-winder/lamination-cutter/)**, or download `index.html` and open it locally. The companion **[Axial Coil Winder](../README.md)** winds the coils onto the finished teeth.

---

## Why stack radially

Flux in an axial-flux tooth runs along the machine axis, so the sheets only have to lie in planes that contain that direction. There are two ways to do that:

- **Tangential stacking** (sheets side by side around the circumference) needs a different trapezoid for each sheet, cut in the plane of the tooth face.
- **Radial stacking** (sheets one behind the other from r₁ to r₂) puts every sheet in the tangential–axial plane. Each one is a **rectangle**, `w(r) × h`, and the slot-leakage flux, which runs tangentially, also stays in the sheet plane.

This tool does the second. Because the sheets are flat and perpendicular to the radius, the inner and outer faces of the tooth are chords rather than arcs. That is the trapezoidal approximation, and on the default stator (r₁ 24, r₂ 40 mm, 12 teeth) it leaves 0.34 mm of iron off the centre of the outer face and gives 99.7% of the ideal sector area.

![The tooth, every sheet drawn](docs/images/view-3d.png)

## Width classes

Tooth width grows linearly with radius, so neighbouring sheets differ by only `2·t·sin α`: 0.052 mm on the default 15° tooth in 0.2 mm sheet. That is finer than the laser kerf, so cutting a different width for every sheet is false precision. Grouping the sheets into **width classes** gives a short list of part types for very little error:

| Classes | Flank error per side | Of each type, per tooth | Of each type, per stator |
|---|---|---|---|
| 80 (one per sheet) | about 0.01 mm | 1 | 12 |
| **16** *(default)* | **0.052 mm** | 5 | 60 |
| 8 | 0.117 mm | 10 | 120 |
| 4 | 0.248 mm | 20 | 240 |

Each class is cut at the width for its mid radius, so the iron area stays the same whichever you choose.

## What it does

- **Builds the tooth from machine dimensions:** r₁, r₂, tooth count, and the tooth/slot ratio, a tooth angle or two measured widths.
- **Cuts one tooth per run.** Parts are nested onto a fixed sheet (200 × 200 mm by default), or the SVG is trimmed to the parts. You run the same file on fresh material once for each tooth: 80 parts and about 6.5 m of cut on the defaults.
- **Compensates for kerf.** Every outline is offset outward by half the beam width.
- **Engraves each part with its class index** as stroked seven-segment digits, so no font has to survive the trip into the laser software.
- **Keeps the rolling direction along the tooth height.** Parts are never rotated in the nest.
- **Pole shoes, optional:**
  - **Integrated:** each sheet is cut as an I-shape.
  - **Separate:** end plates are bonded on after winding, which keeps the needle winder usable.
- **Micro-tabs, on by default:** two small bridges of uncut steel per part, always on an edge with no job in the finished machine.
- **Retention frame, on by default:** a mid-height tab on every sheet, plus a two-piece printed frame that clamps the tabs with no play. It is exported as OpenSCAD, DXF and SVG.

![Retention frame: top view and section](docs/images/view-frame.png)

## Quick start

1. **Stator (panel 01).** Enter r₂, r₁, the tooth count and the tooth height. Choose how the tooth width is defined.
2. **Lamination (panel 02).** Enter your sheet thickness and keep the 16 width classes unless the slot is tight.
3. **Cutting (panel 04).** Measure the kerf on a test piece and enter it. Micro-tabs are on by default, so parts stay in the sheet until you break them out.
4. **Check the Parts and Sheet views**, then **Download SVG**. Red is cut and blue is engrave.
5. **Cut the file once per tooth.** Stack each tooth in index order, class 0 against the inner radius.
6. **Hold the stack.** The retention frame (panel 06) is on by default: download the frame zip and print two halves. Or turn it off and bond the stack instead.

The **[full manual](docs/MANUAL.md)** covers every parameter, the five views, the numbers, cutting practice, the frame and the files in detail.

## Five views

| 3D | Stack | Parts |
|---|---|---|
| ![](docs/images/view-3d.png) | ![](docs/images/view-stack.png) | ![](docs/images/view-parts.png) |
| The tooth, sheet by sheet. Drag to orbit, with dimensions | Looking down the axis: the width-class staircase against the true sector | Every part type, drawn to scale |

| Sheet | Frame |
|---|---|
| ![](docs/images/view-sheet.png) | ![](docs/images/view-frame.png) |
| The nest exactly as it will be cut | One frame half from its mating face, and a section through a tooth (only with the frame on) |

## Output

| File | Contents |
|---|---|
| `laminations_sheet01.svg` | Cut (red) and engrave (blue) layers, in mm. One tooth by default |
| `parts_list.csv` | Every part type: widths, height, quantity per tooth and per stator, radial span, flank error |
| `retention_frame.zip` | `frame_half.scad`, `frame_half.dxf`, `frame_half.svg`: one frame half, to be printed twice |

## Limitations

- **The faces are chords, not arcs.** That is inherent to flat, radially stacked sheets. The flank error is fixed by the width classes; the face error is fixed by the geometry.
- **There is no sheet bonding.** The tool cuts and holds the stack. Varnish, epoxy or VPI is up to you.
- **The nest is simple.** Parts sit in rows and are never rotated, which keeps the rolling direction right but wastes some steel on a large fixed sheet.
- **The frame is a prototype part.** Printed plastic creeps under steady load and heat, so check bolt preload after a run, and move to a bonded or potted stator for anything long-term.

## Licence and author

MIT — see [LICENSE](../LICENSE). [Ozan Keysan](http://keysan.me) · [@ozank](https://github.com/ozank)
