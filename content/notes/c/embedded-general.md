---
title: Embedded Development
linktitle: General Concepts
toc: false
type: docs
date: "2019-12-04T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Embedded C
    weight: 7

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

## Registers

TBC.

---
## Friendly Programming

{{% alert note %}}
**DEF.** When a register have to be modified, _only the bits that are needed should be changed._
```c
P1->DIR &= ~0x02; // change onyl bit 1 in the DIR register of port 1
```
{{% /alert %}}

When modifying registers values on an MCU, _friendly programming_ should be used so that different pieces of code can work on the same register without interfering with each other.

---
## Digital Logic

When controlling a digital device, the ways 0 and 1 are "understood" can be divided in two different ways:

1. **Positive logic**
  * True &rarr; switch pressed &rarr; $3.3 \,\text{V}$
  * False &rarr; switch not pressed &rarr; $0 \,\text{V}$
2. **Negative logic**
  * True &rarr; switch pressed &rarr; $0 \,\text{V}$
  * False &rarr; witch not pressed &rarr; $3.3 \,\text{V}$

In addition, also the voltage range should be consider and will affect correct communication. Some common voltage levels are reported in the following figure.

{{< figure src="../img/digital-logic-levels.png" title="Some of the common logic levels used in ditigal communication. _Figure is taken from TI course slides._" numbered="true" lightbox="true" >}}

In general, the following is true:
$$
V\_{OL} \le V\_{IL} \,\,\text{and}\,\, V\_{OH} \ge V\_{IH} \,\text{for all inputs}
$$
$$
I\_{OL} \ge \sum I\_{IL} \,\,\text{and}\,\, I\_{OH} \ge \sum I\_{IH} \,\text{for all inputs}
$$


{{% alert warning %}}
There are $3.3\,\text{V}$ devices that can accept $5\,\text{V}$ signals on some pins (and so they are called **5V-tolerant**). Unfortunately, this is generally not true, _so be careful to controll the communicating logic used between 2 connected devices._

A wrong connection can damage wither the pin or the entire MCU.
{{% /alert %}}

---
## Resources

* [TI-RSLK Max Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) - Module 6