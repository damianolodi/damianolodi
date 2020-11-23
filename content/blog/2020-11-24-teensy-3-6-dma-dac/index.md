---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "How to Use the DMA on Teensy 3.6 DAC to Generate Waveforms"
subtitle: "My dear CPU, is time to move aside!"
summary: "In some applications, timing and precision are essential. Unfortunately, in those cases, the fact that your code must be processed by the CPU is not desirable. Luckily you can decrease your dependence from the CPU by using *Direct Memory Access* (DMA)."
authors: []
slug: "how-to-dma-teensy-3-6-dac-waveforms"
tags: ["teensy", "dma", "dac"]
categories: ["Embedded Development"]
date: 2020-11-24T07:00:00+01:00
lastmod: 2020-11-24T07:00:00+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Introduction

In some applications, timing and precision are essential. Unfortunately, in those cases, the fact that your code must be processed by the CPU is not desirable, especially if your MCU is doing lots of other things.

Luckily you can decrease your dependence from the CPU by using *Direct Memory Access* (DMA).

In this guide, you will learn how to generate a precise and periodic sawtooth waveform attaching the DMA to the DAC of the Teensy 3.6.

---

## Prerequisites

- **This guide is not for beginners.** I suppose that you already wrote some programs in C/C++ or using the Arduino framework. Some explanations will be provided during the tutorial, but the knowledge of (at least) the following concepts is suggested:
  - GPIO
  - register
  - DAC
- Also, I take for granted that you can organize your code by yourself. In fact, I will provide all the code necessary for the program to work, but will need to adapt it to your use application:
  - you will need to declare functions in the right place
  - to provide more context, I will add pre-processors directives only when required. You should group them at the top of your file/s where needed.
- You need a Teensy *3.6*. You can follow the guide and adapt it to use another Teensy which has a DAC (e.g. Teensy LC), but you may need to change some piece of code.
  - **Unfortunately, the latest Teensy 4.0 and 4.1 have not a DAC so you cannot use them.**
  - [Go to this page](https://www.pjrc.com/teensy/datasheets.html) and download the development manual for the board of your choice. You will its support if you want to understand deeper what you are doing.
- To verify that everything works as expected, you need an oscilloscope...
- ... and you also need to know how to use it.
- To program the board you need VS Code and PlatformIO IDE. As an alternative, you can use the Arduino IDE with the Teensyduino add-on, but I suggest the first option.

---

## Step 1 - Setup DAC0

First of all, you need to set up the DAC, and to do that you need to change some registers.

```cpp
uint8_t dac_setup(uint8_t channel) {
  // Set clock gates for DACs
    SIM_SCGC2 |= SIM_SCGC2_DAC0 | SIM_SCGC2_DAC1;

  // Enable selected channel
    if (channel == 0)
        DAC0_C0 = DAC_C0_DACEN | DAC_C0_DACRFS;
    else if (channel == 1)
        DAC1_C0 = DAC_C0_DACEN | DAC_C0_DACRFS;
    else
        return 1;

    return 0;
}
```

This function will do the following:

1. setup the *System Integration Module* (SIM) to activate the DAC clocks;
2. activate one of the DAC, based on the provided `channel`.

If the channel is not either 0 or 1, the function will return `1` since only two DACs are onboard the Teensy 3.6.

To understand what exactly you are doing with this code you can do two things:

- look at chapters 13 and 41 of the Teensy 3.6 MCU development manual and look for the `SIM_SCGC2` and `DACn_C0` registers. You can also read the whole chapter to (try) understand how the two modules are structured in the microcontroller.
- Using the features of VSCode, place the cursor on the different names that you find and see what their definition are. If you `cmd + click` on the word you will be brought to its definition.

For example, you can see that `SIM_SCGC2_DAC0` is defined as `0x00001000` in the `kinetis.h` file, somewhere in the Teensy libraries. In practice, the first instruction of the code is telling the Teensy to set to 1 the 12th bit (**count starting from 0**) of the `SIM_SCGC2` register.

Then again, if you search the register in the documentation and look at the definition of the 12th bit you will find the following:

> DAC0 Clock Gate Control
>
> This bit controls the clock gate to the DAC0 module.
> 
> - 0 Clock disabled
> - 1 Clock enabled

{{% callout note %}}
If you are at the start of your embedded development journey, exploring the code that you are copy/pasting is a tremendous learning tool.
{{% /callout  %}}


---

## Step 2 - Load the LUT

Now, the DMA if fast because in general it is activated minimizing the CPU usage. To achieve this, you can create a *Look Up Table* (LUT) at the start of the program and read values from there. This will avoid the usage of a variable, that needs to be constantly increased and passed to the `analogWriteDAC1` function.

```cpp
#define LUT_SIZE 8190
uint16_t lut_ramp[LUT_SIZE];

void lut_setup() {
    for (uint32_t i = 0; i < LUT_SIZE; i++) {
        lut_ramp[i] = (i <= LUT_SIZE / 2) ? i : 8190 - i;
    }
}
```

The previous function will generate a global array 8190 position long. Since the DAC of the Teensy is 12-bit in resolution, only values in the range [0, 4095] are valid. Also, you want the ramp to both increase and decrease, so you must fill the first half of the array with [0, 4094] and the second half with [4095, 1].

{{% callout warning %}}
The 0 and the 4095 are filled only a single time since you don't want them to appear twice when the ramp reach the ends.
{{% /callout %}}

---

## Step 3 - Setup DMA

Now it's time to set up the actual DMA, and it can be done using registers. Nonetheless, since the Teensy framework provides a handy library to deal with it (called `DMAChannel`), I will use that instead.

```cpp
#include <DMAChannel.h>

DMAChannel dma1(true);

void dma_setup() {
    dma1.disable();
    dma1.sourceBuffer(lut_ramp, LUT_SIZE * sizeof(uint16_t));
    dma1.transferSize(2);
    dma1.destination(*(volatile uint16_t *)&(DAC0_DAT0L));
    dma1.triggerAtHardwareEvent(DMAMUX_SOURCE_PDB);

    dma1.enable();
}
```

The name of the functions used are mostly self-explanatory, but here are some more details of the general working principle.

1. First of all create a `DMAChannel` object, which represents the actual *DMA* you want to use.
2. Disable the DMA.
3. Define which is the array that is storing all the values that you want to write to the DAC.
4. Define how many bytes each value has. Since we are using `uint16_t`, the corresponding number of bytes is 2.
5. DMA can be used with almost all the modules of an MCU, and until now we have just defined a DMA channel. The `destination` function is telling to the Teensy which module should be attached to this DMA channel.
6. In general, a DMA is triggered by "something", which can be a hardware or software event. Since you want to generate a wavefunction, the DAC must be updated at a constant time interval. In our case is provided by the PDB (more in the next section).

If you want to learn more about the DMA (e.g. the module structure, the precise definition of some words like *channel*, *source* etc.), you can read chapters 23 and 24 of the development manual. You MUST also explore what the various functions of the framework are doing.

By doing so, you can actually dig deeper than what you can learn by just reading the manual. In fact, those two chapters are fundamental but somewhat less detailed than the others.

---

## Step 4 - Enable the PDB

Finally is time to set up the last required building block: the *Programmable Delay Block* (PDB). You can consider it as a precise timer that will trigger an event when it reaches a certain value.

```cpp
#define RAMP_FREQ 2 // Hz
#define PDB_CONFIG                                                   \
    (PDB_SC_TRGSEL(15) | PDB_SC_PDBEN | PDB_SC_PDBIE | PDB_SC_CONT | \
     PDB_SC_DMAEN)

void trigger_clock_setup() {
    SIM_SCGC6 |= SIM_SCGC6_PDB;  // Enable PDB clock

    uint32_t mod = F_BUS / (RAMP_FREQ * LUT_SIZE);
    PDB0_MOD = (uint16_t)(mod - 1);      // Counter between PDB activations
    PDB0_IDLY = 0x0;                     // PDB interrupts delay
    PDB0_SC = PDB_CONFIG | PDB_SC_LDOK;  // Load configuration
    PDB0_SC = PDB_CONFIG | PDB_SC_SWTRIG;
    PDB0_CH0C1 = 0x0101;  // Enable pre-trigger
}
```

I will not explain in detail the effect of every register. I have placed some guide comments and you can read more about it in chapter 44 of the development manual. Nonetheless, I can tell you that **the `RAMP_FREQ` value is the final frequency of the generated ramp waveform** (and you can play with it).

---

## Step 5 - Make everything Work

The `main.cpp` file will look like that:

```cpp
void setup() {
  // DAC
  dac_setup(0);

  // RAMP
  lut_setup();
  dma_setup();
  trigger_clock_setup();
}

void loop() {}
```

Congratulations, you just transformed your Teensy into a waveform generator!

---

## Conclusion 

In this article, you configured a DMA channel of your Teensy to update the DAC0 output and generate a sawtooth waveform. I hope that you got a feel of how to read a development manual, a very useful thing if you want to dig deeper into embedded developments.

If you modify a little bit this program you can create a waveform generator. For example, you can implement the following:

- introduce a serial interface so that a user can interact with the Teensy once the program has started;
- generate various LUT to increase the number of waveforms that one can generate;
- transform `RAMP_FREQ` into a variable so that frequency can be changed.

Have fun!

---

## Other Resources

In the following you can find some resources that I found useful while researching this topic.

1. [Attaching an interrupt to two DMAs - PJRC Forum](https://forum.pjrc.com/threads/54794-Timing-for-running-two-DMA-channels-sequentially)
2. [Using 2 DMA's on two DAC on Teensy 3.6](https://forum.pjrc.com/threads/48457-Using-2-DMA-Channels-for-2-DACs-on-Teensy-3-6) - this was the most helpful in writing this program
3. [Some advice on how to start using DMA by PaulStoffregen](https://forum.pjrc.com/threads/63353-Teensy-4-1-How-to-start-using-DMA)
