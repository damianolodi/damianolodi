---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "How to Change the USB Name of Your Teensy on Linux"
subtitle: ""
summary: "The Teensy is a development board which is gaining quite a lot of popularity. When using it in your project, it can be useful to assign a certain ID to its USB port, so that you can automatically identify and distinguish the device from other peripherals. In this post I will explain how you can change the Teensy USB peripheral name on Linux."
authors: []
tags: ["teensy", "usb", "linux"]
categories: ["Embedded Development"]
date: 2020-10-05T07:00:00+02:00
lastmod: 2020-10-05T07:00:00+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "Wow, both name and surname!"
  focal_point: ""
  preview_only: false

projects: []
---

## Introduction

The Teensy is a development board which is gaining quite a lot of popularity. When using many Teensy boards connected to the USB in one of your projects, it can be useful to assign a unique ID to each of them. In this way, you can identify and distinguish each board.

On the PJRC forum is full of resources explaining how to do it on Windows, but I have not found any resource on how to get a similar result on Linux. In this post, we are going to change the Teensy USB name so that it will be detected by your Linux computer.

## Prerequisites

- A computer with Linux. I am working on Ubuntu 18.04
- A Teensy LC, 3.x or 4.x
- Teensyduino or PlatformIO installed
- Familiarity with C or Arduino development. Ideally, you already have a program that is using the USB serial communication between the Teensy and your computer. If not, [you can use this sample program](https://github.com/PaulStoffregen/USB-Serial-Print-Speed-Test/blob/master/usb_serial_print_speed.ino).
- Familiarity with the command line. In particular, you need to know how to use the `cd` and `ls` commands.

## Step 1 - The `lsusb` Command

`lsusb` is a Linux command that is used to display all the USB peripherals connected to your computer. To test its function, open the terminal and type the command. You will get an output like this one:

```bash
$ lsusb
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 007: ID 046d:c52b Logitech, Inc. Unifying Receiver
Bus 001 Device 002: ID 413c:2003 Dell Computer Corp. Keyboard
...
```

In this project, we need only the output provided by the command without flags, but you can explore its functionality typing `man lsusb`.

The output lists a single USB device in each line and it details the following information:

- *bus ID* and device number on that bus
- **device ID** in the form `vendor_id:product_id`
- human-readable description of the device (if available)

When you connect your Teensy to your computer and call the `lsusb` command, you will find an extra line among all the others (bus and device numbers can vary):

```bash
Bus 001 Device 060: ID 16c0:0483 Van Ooijen Technische Informatica Teensyduino Serial
```

The problem is that, if you connect $n$ Teensy boards (with $n>2$) you will obtain $n$ equal lines. So, if each Teensy has a different program on it, you cannot distinguish them.

## Step 2 - Finding `vendor_id` and `product_id`

The `vendor_id` and `product_id` are values that are set in each Teensy program at compilation time with a `#define` preprocessor directive. In particular, you can find these directives in a file inside the framework that the manufacturer provides you to program the board.

The location of this file changes based on:

1. the board that you need to program;
2. the software that you use to compile your program.

### Teensyduino

If you are using Teensyduino, the exact location depends on where you installed the Arduino software. When you have located it, you need to reach the `cores` directory using the following command

```bash
cd <arduino-installation-path>/hardware/teensy/avr/cores/
```

### PlatformIO

I prefer to use PlatformIO to program my Teensy so that I can use all the features provided by *VSCode*. In this case, the location of the `cores` directory can be reached using the following command:

```bash
cd ~/.platformio/packages/framework-arduinoteensy/cores/
```

### Selecting the Correct Subdirectory

Based on the board you want to program, you need to `cd` into:

- `teensy3` for Teensy LC and Teensy $3.x$
- `teensy4` for Teensy $4.x$

## Step 3 - Changing `vendor_id` and `product_id`

Now that you found the right path for your board, you need to change the `VENDOR_ID` and `PRODUCT_ID` definitions in the `usb_desc.h` file.

You can do that using *nano* (or whichever text editor you prefer) by typing `nano usb_desc.h`. Then, find the following lines and change them as you prefer.

```c
#define VENDOR_ID             0x16C0
#define PRODUCT_ID            0x0483
```

Keep in mind the following:

1. you need to place a *4-digit hexadecimal number*;
2. if you insert a `VENDOR_ID` that is already assigned, your device will be recognized as that one. This is not important if you just need to recognize the Teensy in your project.

Now save and exit from *nano* typing `ctrl+X` and `Y`.

## Step 4 - Compiling and Uploading

Finally, you can recompile and upload your program as you always do. Next time you upload correctly, your board will not be a Teensy anymore!

As much as it is exciting, changing the Tensy USB name has a downside: when you try to upload a program to a *no-more-Teensy*, Teensyduino will not recognize it and will fail to flash the binary. Luckily, you just need to press the button that you find on the board to make it work.

## Conclusion

Following this article you learned:

- where the *magical* Teensy files are located;
- how to change the USB name of the Teensy so that it can be identified on Linux systems;
- discovered a new terminal command, `lsusb`.

Now you are ready to create your next mega project employing $n>2$ Teensy boards... have fun!
