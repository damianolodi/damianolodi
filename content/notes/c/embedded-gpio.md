---
title: General Purpose Inputs/Outputs
linktitle: GPIOs
toc: false
type: docs
date: "2020-01-02T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Embedded C
    weight: 7

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

{{% alert warning %}}
Names on this page are correct for the MSP432, but lots of naming conventions are similar for all MCUs manufacturers.
{{% /alert %}}

GPIOs are used to generate _binary electric signals._

All the pins on an MCU are _grouped in **Ports**_ (GPIOPort1, GPIOPort2, ...), and almost each of them can be used as a GPIO or with an alternate funcition (ADC, Timer,...) which is specified by the documentation.

In general, most of the ports of an MCU are 8-bit wide, so that an 8-bit register is associated to them and it is used to select a pin function durin initialization.

| Port Name                       | Function         |
| --------------------------------| ---------------- |
| P6.0 / A15 _(port 6 pin 0)_     |  GPIO or ADC     |
| P2.7 / TA0.4 _(port 2 pin 7)_   |  GPIO or timer   |
| P3.0 / UCA2STE _(port 3 pin 0)_ |  GPIO or serial  |

---
## Negative and Positive Logic

When controlling a digital device, two different "communication protocols" are possible:

1. **Positive logic**
  * True &rarr; switch pressed &rarr; $3.3 \,\text{V}$
  * False &rarr; switch not pressed &rarr; $0 \,\text{V}$
2. **Negative logic**
  * True &rarr; switch pressed &rarr; $0 \,\text{V}$
  * False &rarr; witch not pressed &rarr; $3.3 \,\text{V}$

---

