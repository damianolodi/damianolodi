---
title: Finite State Machines
linktitle: Finite State Machines
toc: false
type: docs
date: "2019-01-03T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Embedded C
    weight: 10

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7

diagram: true
---

{{% alert note %}}
**DEF.** Finite state machines are a set of inputs ~~(from sensors)~~, outputs ~~(to actuators)~~, states and transitions. The associated graph defines the I/O relationships.
{{% /alert %}}

Finite state machines (FSM) is an abstractive design technique which will make the program (1) faster to develop, (2) easier to debug and (3) easier to change.

The concept of _abstraction_ in a program refers also to the usage of logical names rather than using registers syntax (e.g. `P1->W = ...`). The two things are usually linked using `#define`.

---
## FSM Types

There are two types of FSM: **Moore FSM** and **Mealy FSM.** They are compared in the followinf table.

| **Moore FSM** | **Mealy FSM**  |
| --------------| -------------- |
| Output value depends on **current state.** | Output value depends on **input and current state.** |
| The _state_ determines the output | The _state transition_ determines the output. |
| The output determines what to do while in that state. | The output determine how to change state.[^1] |

---
## FSM Rules

1. Simple structure: input &rarr; process &rarr; output.
* Information is encoded by being in a state.
* FSM controllers are simple (e.g. _output, wait, input, go to next state_).
* Complexity is captured in the state graph.
* There is a 1-1 mapping between state graph and software implementation.

{{< hl >}}**DEF.** A **state** is the description of the current condition of the system{{< /hl >}} (_what the system belive it is true_).

{{< hl >}}**DEF.** A **controller** is a software that inputs, outputs and changes state based on the state graph.{{< /hl >}}

---
## FSM Design

Every state in a FSM has:

* name;
* output value;
* wait time;
* arrows that connect it to other states (including itself).

_A FSM can be designed both in **table** and in **graph**._ The two representation are equivalent and one can choose what is best for the project. In general, if the number of states is really high the table representation is less confusing.

| State/Input | 00 | 01 | 10 | 11 |
|-------------|----|----|----|----|
| goN(100001, 30) | goN | waitN | goN | waitN |
| waitN(100010, 5) | goE | goE | goE | goE |
| goE(001100, 30) | goE | goE | waitE | waitE |
| waitE(010100, 5) | goN | goN | goN | goN |

{{< figure src="../img/fsm-example.png" title="Example of FSM table and graph representation. _Both examples are taken from the TI-RSLM Max course._" numbered="true" lightbox="true" >}}

Tables can easily be translated into code using structures.
```c
const struct State {
    uint32_t Out;       // 6-bit output
    uint32_t Time;      // 1ms units
    uint32_t Next[4];   // list of next states
};

typedef const struct State State_t; // it can be combined in a single instruction

#define goN     0 // 00
#define waitN   1 // 01
#define goE     2 // 10
#define waitE   3 // 11

State_t FSM[4] = {
    {0x21, 30000, {goN, waitN, goN, waitN}},
    {0x22,  5000, {goE, goE, goE, goE}},
    {0x0C, 30000, {goE, goE, waitE, waitE}},
    {0x14,  5000, {goN, goN, goN, goN}}
}

void main(void){
    uint32_t cs;        // index of current state
    uint32_t input;
    Traffic_Init();     // init ports and timer
    cs = goN;
    while(1){
        // set appropriate output
        P4->OUT = (P4->OUT & ~0x3F) | (FSM[cs].Out);
        // wait appropriate amount of time
        Clock_Delay1ms(FSM[cs].Time);
        // read current input
        input = (P5->IN & 0x03);
        //switch state based on input
        cs = FSM[cs].Next[input];
    }
}
```

{{< hl >}}In this way the complexity grows in the graph but not in te software, one only has to add rows to the FSM definition!{{< /hl >}}

### Using Pointers

As a reference, I report also the example which implements pointers. This code will run a bit faster because now I initialize as a full 32-bit pointer instead of a 2-bit number.

```c
typedef const struct State {
    uint32_t Out;       // 6-bit output
    uint32_t Time;      // 1ms units
    uint32_t Next[4];   // list of next states
} State_t;

#define goN     &FSM[0] // 00
#define waitN   &FSM[1] // 01
#define goE     &FSM[2] // 10
#define waitE   &FSM[3] // 11

State_t FSM[4] = {
    {0x21, 30000, {goN, waitN, goN, waitN}},
    {0x22,  5000, {goE, goE, goE, goE}},
    {0x0C, 30000, {goE, goE, waitE, waitE}},
    {0x14,  5000, {goN, goN, goN, goN}}
}

void main(void){
    State_t *pt;        // pointer to current state
    uint32_t input;
    Traffic_Init();     // init ports and timer
    pt = goN; // index of current state
    while(1){
        // set appropriate output
        P4->OUT = (P4->OUT & ~0x3F) | (pt->Out);
        // wait appropriate amount of time
        Clock_Delay1ms(pt->Time);
        // read current input
        input = (P5->IN & 0x03);
        //switch state based on input
        pt = pt->Next[input];
    }
}
```

---
## Resources
* [TI-RSLK Max Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) - Module 7


[^1]: _Not yet really understand what does that means._