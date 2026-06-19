---
description: >-
  This page describes how to control an unmanned vehicle with SBUS using a UXV
  controller
tags:
  - add-later
---

# Configuring an Unmanned Vehicle with SBUS

{% hint style="info" %}
This guide assumes a working connection from [previous](wiring-for-control-of-unmanned-vehicle-using-sbus.md) guide.
{% endhint %}

## Part List:

* 1x UXV Micronav Dev Kit (or another controller with SBUS output) + GCS Radio
* Finished car with connection from [previous](wiring-for-control-of-unmanned-vehicle-using-sbus.md) guide

## Optional Tools List

* Multimeter
* Oscilloscope (very useful)

## 1. UART Output from Micronav

The Micronav controller supports UART output to the Doodle Labs radio. If you have a radio connected (SRM), check if the SBUS (form of UART, check the [guide](<../README (2).md#sbus>)) output is enabled in NavSuite. Navigate to Output, where SBUS EABLED should be displayed.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 014656 (1).png" alt=""><figcaption></figcaption></figure>

If SBUS is disabled, unlock the NavSuite settings by clicking on the lock icon in the top right of the screen and type in the entry code.

Then click on ENABLE SBUS to enable SBUS output from the controller. Also click on SBUS AIRSIDE, which will enable failsafe values and a protocol, which will make the SBUS output from the Airside cleaner.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 014741.png" alt=""><figcaption></figcaption></figure>

Then proceed to the SBUS tab, which is found at the top center of the screen. Here you can set the serial protocol settings, or configure failsafe behavior. UXV is using the following settings:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 014805.png" alt=""><figcaption></figcaption></figure>

These settings will make the connection work for the wiring described in the previous guide.&#x20;

It is useful to verify that the signal is propagating through the system correctly after every step. <mark style="color:$danger;">UXV used an oscilloscope to probe the pins on the Micronav controller to verify the correctness of the connection. (where to find the documentation for the pinout when using SRM?)</mark>

## 2. Doodle Labs Configuration

This guide assumes a working connection is already established between a pair of Doodle Labs radios and only enabling serial configuration is nescessary.

Since the radios communicate over IP, the UART signal is split into individual packets, which are sent over the network and decoded on the other side. Doodle Labs is using socat utility for this.&#x20;

{% hint style="info" %}
There is a known bug in socat, which makes changing the configuration through the command line interface glitchy. Doodle Labs reccomend users to use the web GUI for socat configuration.
{% endhint %}

The Doodle Labs radio is es

A correct configuration for the GCS Radio is the following:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 115417.png" alt=""><figcaption></figcaption></figure>

And respective configuration for the UxV radio:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 115530.png" alt=""><figcaption></figcaption></figure>

