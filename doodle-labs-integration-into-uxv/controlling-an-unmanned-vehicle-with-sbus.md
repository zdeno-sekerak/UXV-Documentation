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

The Micronav controller supports UART output to the Doodle Labs radio. If you have a radio connected (SRM), check if the SBUS (form of UART, check the [guide](../hardware-selection/electronics-communication-basic-concepts.md#sbus)) output is enabled in NavSuite. Navigate to Output, where SBUS EABLED should be displayed.

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
There is a known bug in socat, which makes changing the configuration through the command line interface glitchy. Doodle Labs recommend users to use the web GUI for socat configuration.
{% endhint %}

{% hint style="info" %}
Always start by setting Simple Configuration - Simple Configuration settings first. The configuration acts as a script which will erase all other conflicting settings, if set differently to the simpleconfig. The baud rate settings for in the simpleconfig are only&#x20;
{% endhint %}

Navigate to Advanced Settings - Services - Serial Configuration and enable socat on both of the radios. Then continue to set the settings according to the screenshots below.

A correct configuration for the GCS Radio is the following:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 115417.png" alt=""><figcaption></figcaption></figure>

And respective configuration for the UxV radio:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 115530.png" alt=""><figcaption></figcaption></figure>

A radio can be either set as a socat server, or socat client. When a radio is set as a client, it initiates the connection to the radio set up as a server (host IP address). It has to be configured with the exact IP address of the radio it wishes to reach and it will try to do so. On the other hand the server just waits for any radio to reach out, without listening to only specific address, but it is connected to a specific port.&#x20;

For bench testing, it does not matter which of the radios is set as a server or a client. In the real world however, it is typical to assign the radio, which is more likely to suffer a power outage as a client.&#x20;

{% hint style="info" %}
The settings of socat server/client and simpleconfig UAV/GCS will not conflict. They are operating on different layers of the OSI model.
{% endhint %}

The devices are configured as TCP, which is a networking protocol with higher latency and lower data throughput, but lower loss rate, because it checks the packets it sends and waits for them to be received. On the other hand UDP does not check the packets and if there is a problem it just keeps sending them, which is for example favorable for video transmittion, but not so much for sending control commands.&#x20;

The port in the socat configurations of both the GCS and UxV is set to 2001. By default the port is 2000, however the setting conflicts with the simpleconfig of the UAV radio, where it also searches for GCS at port 2000. As one port cannot be used for two operations, socat would not work with the default settings.

{% hint style="info" %}
The most tricky part of this configuration is avoiding conflicting settings. A simple way to discover them is to power off and on the radios and check socat. If is it disabled even if it was previously enabled, there is likely a conflicting setting.
{% endhint %}

I also have to configure the firewall traffic rules to the new port. Currently it is set to allow socat on port 2000, I need to change it to port 2001.
