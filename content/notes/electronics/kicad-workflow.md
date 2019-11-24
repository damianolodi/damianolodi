---
title: PCB Design Workflow
linktitle: Workflow
toc: false
type: docs
date: "2019-11-23T00:00:00+01:00"
draft: false
menu:
  electronics:
    parent: KiCad
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## Schematic Design Workflow

1. Set grid and page settings.
- Place all symbols on the page.
- Arrange all symbols on the page (without connecting them).
- Annotate the schematic.
- Associate footprints to all symbol libraries.
- Wire every element and assign power flags to power symbols.
- Assign custom names to (at least some) nets, so that connections understanding is simplified.
- Regularly run ERC (Electrical Rules Check).
- Place text comments on the schematic and divide in blocks to simply understanding.
- Export netlist.

### General Tips \& Tricks
- If possible, the convention says to place inputs on the left, outputs on the right.
- Arrange symbols in functional blocks.
- Don't mix EU and US symbols, stick only to a single convention.

---
## PCB Layout Workflow
1. Set page settings (title, author,...)
- Set grid and design rules:
  - number of layers and types;
  - global design rules based on PCB manufacturing constraints;
  - net class widths (usually 1 big and 1 small);
  - set text and graphics parameters;
  - define tracks and vias width so that they are present in the top toolbar menus.
- Define the board outline based on design constrains (**edge.cuts layer**).
- Place mounting holes (if necessary).
- Place all footprints on the board thinking to (3D) dimensional constraints and necessary connections. Use _Align/Distribute_ to organize the placement.
- Then, lock all footprints in place.
- Use _Align/Distribute_ to organize
- Route all the tracks:
  - _start with critical tracks_ (e.g. the ones that requires certain lengths for impedance matching);
  - route power traces;
  - place all the remaining tracks.
- Add copper fills.
- Complete silkscreen artworks:
  - add _name \& version_ of the board;
  - place _logo \& artworks_;
  - annotate other instructions that may help the end-user (e.g. components names, connectors pin name, components values,...).
- Apply DRC (Design Rules Check).
- Generate Gerber and drill files.

### General Tips \& Tricks
- If possible, _keep the board shape rectangular_ (this simplify mounting and decrease production cost).
- _Take care of high power/high-temperature components_ (look at the datasheet to accommodate enough space and heatsink).

#### Traces
- Keep traces _as short as possible_:
  - this will decrease resistance to current flow and minimize energy loss in the form of heat;
  - trace length affect also signal propagation time (very important for high-speed applications).
- **Avoid sharp angles**. They can generate interferences due to signal reflection (especially true for application at frequencies higher than $200\,\text{MHz}$). _Use $45^{\circ}$ angles or rounded edges when changing track direction_.
- _Increase copper weight only when necessary_: it improve electrical characteristics of the board but it increases also etching costs.
- **Increase track width for high power/high current lines**. _Use the KiCad width calculator to find the right dimension_.
- Always set correct tracks separations:
  - consider always _manufacturing limits_ and _interference limits_ (crosstalk in high-frequency applications);
  - $0.15 \text{mm}$ is good for every manufacturer nowadays.