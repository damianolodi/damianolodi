---
title: Voltage Regulators
linktitle: Voltage Regulators
toc: false
type: docs
date: "2020-01-02T00:00:00+01:00"
draft: false
menu:
  electronics:
    parent: Components
    weight: 1

# Prev/next page order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

_A voltage regulator is an IC designed to maintain a costant output voltage_ $V_ {out}$ _from a variable input power source (_$V_ {in}$ _and_ $I_ {in}$_) and for a variable circuit load_ $I_ {out}$. For this reason, {{< hl >}}voltage regualtors are used every time a clean and constant voltage is required.{{< /hl >}}

Mainly, one can have 3 sources of power:

1. **wall** ($120 / 240 \, V_ {AC}$ and $50 / 60 \, \text{Hz}$)
  * In this case, one needs to convert AC to DC and clean the input voltage to obtain a constant output power;
2. **DC power supply** ($+5\,\text{V}$ on USB, $+12\,\text{V}$ in automotive).
  * A regulator is needed to step down to $+3.3\,\text{V}$;
3. **battery**.
  * A regulator is needed to generate a constant voltage while batteries are discharging. 

---
## Types of Regulator

There are different types of regulators, and each one is suited for a different application.

### Linear Regulators

{{< figure src="../img/linear-regulator.png" title="Linear regulator example circuit" numbered="true" lightbox="true" >}}

In this circuit, a variable resistor will adapt to $V_ {in}$ and $I_ {out}$ to have a constant $V_ {out}$. In general one has to consider that
$$
V_ {in} > V_ {out} + V_ {DO} \,\text{(dropout voltage)}
$$

An example of linear voltage regulator is the _TI 7805_.

#### Pros and Cons
* This components are usually **simple** and **cheap**.
* Linear voltage regulators are **low-noise** components.
* Linear voltage regulators are  **inefficient**. In fact, $I_ {in} = I_ {out}$ but $V_ {in} > V_ {out}$, so it becomes stressful for the battery, e.g.
$$
7.2 \,\text{V} \cdot 100 \text{mA} - 5 \text{V} \cdot 100 \,\text{mA} = 0.22 \,\text{W of power loss} 
$$
which means that the component will become hot!

### Switching Regulators

Switching regulators comes in 3 different types, depending on the relation between $V_ {in}$ and $V_ {out}$. The efficency of a regulator can be expressed using the following equation:
$$
\frac{V_ {out} \cdot I_ {out}}{V_ {in} \cdot I_ {in}}
$$

Example circuits are reported in the following images.

{{< figure src="../img/switching-regulator-buck.png" title="**Buck** switching regulator example circuit - $V_ {in} > V_ {out}$" numbered="true" lightbox="true" >}}

{{< figure src="../img/switching-regulator-boost.png" title="**Boost** switching regulator example circuit - $V_ {in} < V_ {out}$" numbered="true" lightbox="true" >}}

{{< figure src="../img/switching-regulator-buck-boost.png" title="**Buck-boost** switching regulator example circuit - Both relations are possible" numbered="true" lightbox="true" >}}

#### Pros and Cons

* Switching regulators are **more efficient** than linear ones, because $P_ {in} \sim P_ {out}$
* Switching regulators are also **more expensive**
* They are also **more noisy**, due to the presence of EM field generated from their working principle



---
## References

* [TI-RSLK Max Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) - Module 5