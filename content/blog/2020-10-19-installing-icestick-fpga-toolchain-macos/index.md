---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "How to Setup the Icestorm Toolchain on macOS"
subtitle: "First step in to develop FPGAs like a PRO!"
summary: "I have recently experienced the so-called FPGA Hell... very, very distressful condition! In this guide, you will learn how to set up the Icestorm toolchain to program the Icestick on macOS. When you are finished, you will be able to understand the basics of the compilation process and you can start to adapt it to your particular workflow."
authors: []
slug: "how-to-setup-icestorm-toolchain-icestick-macOS"
tags: ["fpga", "toolchain", "macos", "icestick"]
categories: ["Embedded Development"]
date: 2020-10-19T07:00:00+02:00
lastmod: 2020-10-19T07:00:00+02:00
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

I have recently experienced the so-called [FPGA Hell](https://zipcpu.com/blog/2017/05/19/fpga-hell.html): you are completely stuck on a Verilog development problem, and you have no clue where to find the bug... very, very distressful.

Luckily, this problem pushed me to explore the Verilog compilation process a little deeper. At work, we use APIO to program Lattice FPGAs, but I always feel that something is missing when using "pre-packaged" tools: what I am really doing?

In this guide, you will learn how to set up the Icestorm toolchain to program the Icestick on macOS. When you are finished, you will be able to understand the basics of the compilation process and you can start to adapt it to your particular workflow.

> :warning: macOS is probably not the most friendly OS for FPGA development. It is possible to make everything work but **CAUTION: it can be painful.**

## Prerequisites

- A computer with macOS (the process is probably very similar if you are using Linux, but I have not tested it yet).
- Familiarity with the command line. You need to know how to use `cd`, `ls` and a text editor (`nano`).
- You need to be familiar with the concept of `PATH` and the `.bashrc` file.
- You will need *[Homebrew](https://brew.sh/)* to install some software
- Familiarity with *git* is requested.
- Finally, you need the [Icestick](https://www.latticesemi.com/icestick) to test your installation

## Step 1 - Downloading the Toolchain

There are many ways of installing the toolchain, but I have recently discovered the `fpga-toolchain` [page on GitHub](https://github.com/open-tool-forge/fpga-toolchain). This project creates daily builds of the toolchain for every OS.

So [head over to the project release page](https://github.com/open-tool-forge/fpga-toolchain/releases) and download the proper archive: for macOS, you should download the one for the Darwin architecture.

> :warning: If you are reading this from the future (a future in which Mac computer switched Intel CPUs with Apple ARM architecture) this last choice may not apply to your particular case!

Now you have to unzip it and finding a place on your computer in which to store the `fpga-toolchain` directory. Personally, I (*try to*) use the PARA productivity system, so I am storing this into `~/Documents/resources/fpga/`.

## Step 2 - Adding to the PATH

If you explore the directory content you will find the `bin/` subdirectory, which contains loads of executable. Among all those programs, there are the ones that you need to program the Icestick:

- `yosys` &rarr; synthetize Verilog code (i.e. convert the HDL description to gate-level representation);
- `nextpnr` &rarr; a software that decide where to place the logic elements in the floorplan of the FPGA and how to route connections (*pnr* stays for *place and route*);
- `icepack` &rarr; takes the *pnr* output and produce the binary bitstream;
- `iceprog` &rarr; program the Icestick with the `.bin` file.

and some other utilities, all part of the Icestorm project.

Nonetheless, we need to tell the OS where to find all this program. To do that, you need to modify the `.bashrc` file in your home folder. Open *Terminal*, type `nano ~/.bashrc` and add the following line to the bottom of the file

```bash
# Add FPGA toolchain to path and add GTKwave
export PATH="/Users/<user>/<your-storage-path>/fpga-toolchain/bin/:$PATH"
```

You need to fill `<user>` and `<your-storage-path>` with the proper values. In my case, this second value is equal to `Documents/resources/fpga/`. In the end, type `ctrl + X` and `Y` to close and save the file, then re-launch the *Terminal* app.

## Step 3 - Install FTDI Drivers

You have now installed the tools needed to program your Icestick. Unfortunately, since the board USB interface is managed by an FTDI chip, you need to install FTDI drivers.

To do that, [head over to this download page](https://www.ftdichip.com/Drivers/VCP.htm), click on the link reported in the table corresponding to the row *Mac OS X 10.9 and above* and install the drivers using the downloaded `.dmg` file.

### Solving the FTDI Drivers Bug

Unfortunately for macOS users, FTDI drivers have a bug which can prevent the detection of the Icestick. The bug and its resolution is [reported on the *Icestorm project* page](http://www.clifford.at/icestorm/notes_osx.html), but you have to apply these steps **every time you reboot your system.**

Since this can be tedious and hard to remember, I find that the best way is to create an `alias` to quickly apply all the steps at once, when needed.

To do that, open the *Termianl* app and type `nano ~/.bashrc` again. At the bottom of the file, type the following lines:

```bash
# Load and unload FTDI
alias loadftdi="kextstat | grep FTDIUSBSerialDriver && sudo kextunload -b com.FTDI.driver.FTDIUSBSerialDriver && kextstat"
```

Again, type `ctrl + X` and `Y` to close and save the file, then re-launch the *Terminal* app.

From now on, in your command line you can type `loadftdi`, input your system password when required, and solve the FTDI driver loading problem.

## Step 4 - Install Other Useful Software

Technically, all the software needed to build and program the Icestick is now installed. Nonetheless, some other tools can be useful when developing code in Verilog.

To install the additional software, you should open *Terminal* and type

```bash
brew install icarus-verilog
...
brew install make
...
brew cask install gtkwave
```

You have installed the following program:

- `icarus-verilog` &rarr; synthetize Verilog code for simulation;
- `gtkwave` &rarr; a GUI program useful to visualize the signals generated from the simulation;
- `make` &rarr; you can think to this program as the glue of the whole toolchain.

Unfortunately, also this software require some care to work properly.

### Using the Correct `make`

It can happen that for some reason you already have `make` installed on your system. You need to tell macOS which one to use, and to do that you need to update the `PATH` variable.

So open *Termianl*, type `nano ~/.bashrc` and add this line to the bottom of the file:

```bash
export PATH="/usr/local/opt/make/libexec/gnubin:$PATH"
```

> The exact path that you must add may change over time. Luckily, `brew` output after the installation will tell you exactly which is the correct string that you need to paste!

Again... type `ctrl + X` and `Y` to close and save the file, then re-launch the *Terminal* app. To test that you are using the right software, type `which make` in the *Terminal* and check that the output match the string you placed in the `./bashrc` file.

### Installing *gtkwave* CLI

For some reason that I do not understand, `gtkwave` does not work properly when invoked throught the command line or using `make`.

I have spend so much time trying to make it work, that now I do not really remember all the steps I have made. Nonetheless, this resources can help:

1. [follow this guide](https://ughe.github.io/2018/11/06/gtkwave-osx) to install `gtkwave` command line tools;
2. [follow this other guide](http://labnote.me/gtkwave-macos-commandline-param/) when changing the *Makefile* used to compile your program. In particular, read the *Adding Alias* and *Makefile* paragraphs.

## Step 5 - Testing the Installation

The purpose of this article is not explaining how `make` works and how to use *Makefile* to compile your project. Nonetheless, if you are here I bet that you are interested in FPGA development and you want to try if the toolchain works.

To do that, I suggest that you follow these steps:

1. using *git*, [clone this awesome set of tutorials](https://github.com/Obijuan/open-fpga-verilog-tutorial) by Obijuan.
    - By the way, you can use these tutorials [documentation](https://github.com/Obijuan/open-fpga-verilog-tutorial/wiki/Chapter-0%3A-you-are-leaving-the-private-sector) to learn FPGA development. Some of them are in Spanish, but Google Translate is magic.
2. reach the first project using `cd <your-clone-directory>/open-fpga-verilog-tutorial/tutorial/ICESTICK/T01-setbit`
3. using a text editor, change the content of the *Makefile* in this way:
   - add `gtkwave:=/Applications/gtkwave.app/Contents/Resources/bin/gtkwave` in the first line
   - apply the following changes to the simulation stage

    ```bash
    #-- Ver visualmente la simulacion con gtkwave
    $(gtkwave) setbit_tb.vcd setbit_tb.gtkw
    ```
  
   - apply the following changes to the building stage

    ```bash
    #-- Sintesis
    yosys -p "synth_ice40 -top setbit -json setbit.json" setbit.v

    #-- Place & route
    nextpnr-ice40 --hx1k --pcf setbit.pcf --json setbit.json --asc setbit.asc

    #-- Generar binario final, listo para descargar en fgpa
    icepack setbit.asc setbit.bin
    ```

Keeping fingers crossed, type `make` in the command line. Then start debugging what is not working (**PAINFUL, remember?**)

## Conclusion

In this article, you set up the toolchain needed to program the Icestick (and other Lattice FPGAs). You had also your first contact with *build systems* (i.e. `make`), which I hope to explore more in future articles. Now you can start developing FPGAs like a pro!
