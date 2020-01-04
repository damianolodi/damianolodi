---
title: Debugging Real Time System
linktitle: Debugging RTS
toc: false
type: docs
date: "2019-01-04T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Embedded C
    weight: 11

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

{{% alert note %}}
**DEF.** An _instrument_ in software is a piece of code that one can inject in the codebase for the purpose of debugging.

**DEF.** A _dump instrument_ is a debugging tool that read a value and store it in a variable/array. It is similar to `printf`, but one can observe the behaviour inside the debugger.
{{% /alert %}}

Debugging _instruments_ can be both good and bad, and can be classified based on how muche the code is affecting the parameters that one needs to measure. This property is called **intrusiveness.** {{< hl >}}An instument is _minimally intrusive_ if $t/\Delta t$ is small, where $t$ is the time spent to execute the insturment and $\Delta t$ is the time passing between executions.{{< /hl >}}

There could be 2 variatons of _dump instruments:_

1. **continuous** &rarr; saves the last $n$ values acquired;
2. **filter** &rarr; saves only certain conditions to reduce the amount of data observed (e.g. record a value only if it changes).

```c
    /** Example taken from the TI-RSLK MAX curriculum for the MSP432 **/
    // Continuous dump instrument
    uint16_t Buf[32];
    uint32_t I = 0;
    void Record(uint16_t x) {
        Buf[I] = x;
        I = (I + 1) & 0x1F;
    }

    // Filter dump instrument
    void Record2(uint16_t x) {
        if (P1->IN & 0x01) {
            Buf[I] = x;
            I = (I + 1) & 0x1F;
        }
    }
```

---
## Performance Debugging

The point of debugging is mainly to answer 3 simple questions about a piece of code:

1. _where_ is it executing?
2. _when_ is it executing?
3. _How long_ does it take?

### Triple Toggle Technique

The idea is to change status 3 times to read both (1) _body execution_ and (2) _time between interrupts._

{{< figure src="../img/triple_toggle_technique.png" title="Example of signal on the oscilloscope generated using the _triple toggle technique._" numbered="true" lightbox="true" >}}

```c
#define LED (*((volatile uint8_t *) (0x42098068)))
void ISR(void) { // stands for "interrupt service routine"
    LED ^= 1;
    LED ^= 1;
    // body - my code
    LED ^= 1;
}
```
### Bit-Banding

In the previous example, the instruction `P2->OUT` was replaced by a bit-banded value. In fact, each pin of a Cortex M3 MCU is associated to a certain value, which can be modified by a _Read&rarr;Modify&rarr;Write_ operation. To calculate this value one can use the following procedure:

- `P2->OUT` is identified by the value 0x40004C03. Let's take the bottom part of the number and define $n=0x4C03$.
- Let's suppose that one wants to write on pin P2.2, so let's define $b=2$.
- Finally solve the following equation
$$
\text{Bit-banded address} = 0x42000000 + 32 \cdot n + 4 \cdot b
$$

---
## Programming flash memory

Sometimes it can be useful to write values inside the flash memory to free some RAM and to save values also if power is turned off (e.g. for device settings). The writing operation takes roughly $\sim \text{ms}$ time, quite a bit more than a normal RAM writing operation.

This can usually be done simply using some functions. On the MSP432, those functions are _Flash\_Erase_ and _Flash\_Write\_Array_.

### Pros and Cons

1. High density
- Erase to 0xFF
- Program zeros
- Slow to erase
- Slow to program
- Fast to read

{{< figure src="../img/flash-memory-working-principles.png" title="Working principles of a single bit of a flash memory." numbered="true" lightbox="true" >}}

---
## Resources

* [TI-RSLK Max Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) - Module 10

