---
title: Custom Symbols and Footprints
linktitle: Footprints
toc: false
type: docs
date: "2019-11-23T00:00:00+01:00"
draft: false
menu:
  electronics:
    parent: KiCad
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3

##### AGGIUNGI
# procedimento per creare simboli custom
# preocedimento per creare custom logo
---

## Custom Symbol Workflow

1. ...

---
## Custom Footprints Workflow
1. Draw the outline of the package in the _F.fab_ layer, as reported on the proper datasheet.
- Place the pads and edit their properties. Control that their placement is correct as reported on the datasheet.
  - Add NPTH-mechanical hole if necessary.
- Place minimal graphics and text on the _F.SilkS_ layer.
- Place the outer boundaries of the component in the _F.CrtYrd_ layer. This is required both for KiCad DRC and for visual reference on where to safely place components to avoid overlap.

### General Design Guide
- For SMD components, the origin of the grid should be in the center of the footprint. For THT components, usually the pin 1 is placed in the origin instead.
