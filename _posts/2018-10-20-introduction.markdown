---
layout: blog
title:  "Introduction"
date:   2018-10-20 14:30:00 +0300
# categories: en
---

I'll explain my choices for the communication interfaces, and the architecture in general.

# The problem

There is no <abbr title="Both intuitive and affordable">easy</abbr> solution for a student who wants to visualize and correlate something physical automatically, without code.

# The solution

A series of sensors, which connect via a combination of [BLE](#ble-bluetooth-low-energy)/[USB](#usb)/[UART](#uart-serial)/[LVDS](#lvds-low-voltage-differential-signaling) to either a PC or a data logger.

## Prototype hardware

Currently I'm prototyping using the Arduino Nano connected to a PC. This lets me test the USB + UART communication methods.

Later I will test BLE with the ESP-WROOM-32.

![Prototype Board](/res/blog/introduction/proto.jpg)

The wires you see coming from the Arduino nano are +5v, GND, TX, and RX. These can be used to connect to a data logger that acts like a PC and sets up the data stream itself.

## Closer-to-production hardware

I designed and ordered some host boards for the ATmega8A-AU chip, the same chip used in the Arduino Nano.

It has some optional analog smoothing components and some pads for SMD components on the back.

You can either solder the sensor on the SMD pads on the back or connect them on a seperate PCB (or Proto board). You also need to supply the board with 3-5v somehow, that's up to the sensor manufacturer.

![Prototype PCB](/res/blog/introduction/bb0.jpg)

I will likely switch to ARM Cortex M0+ processors as those are cheaper for their performance and I/O.

## The interfaces

### BLE (Bluetooth Low Energy)
This is for sensing and controlling in remote areas (ie. submerged underwater), and for measuring on iOS devices (because of MFI restrictions). (Obviously can be used on other OSs).

The protocol is the same as an HM-10.

### USB
A simple UART to USB converter is used, and can be used with a PC or an Android phone with OTG.

### UART (Serial)
Used to connect to data loggers or custom hardware (eg. robotics controllers?)

### LVDS (Low Voltage Differential Signaling)
A pair of LVDS converters at each end turn a UART line (which has to be pretty short), into a robust and capable long distance line. Won't be used by most hobbyists, but can be used to measure from multiple sensors that are far apart into a single data logger.

## The software
Should allow plug and play detection of sensors and data loggers, should automatically try to find correlations between data (or data to time), and display it nicely.

Will be written in C# using the Xamarin framework and will support UWP > Android > Linux > WPF > iOS? > MacOS? in that order.

Apple environments are a question mark because of their restricted nature, and the fact I don't own a Mac.

The bulk of the logic will be written as a portable C# library as thus will be portable right away.

## The MVP (Minimal Viable Product)
My MVP will be two Arduino Nanos, each as a different sensor (perhaps light and ultrasonic), each connected via USB to a Windows PC.

The software will display each connected device, it's immediate value, and a value/time graph of it's value.