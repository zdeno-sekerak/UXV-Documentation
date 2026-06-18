---
description: >-
  This page explains how to wire a connection for an unmanned vehicle, which
  will be controlled using SBUS. It is a precourse for the following guide,
  which explains the configuration.
---

# Wiring for Control of Unmanned Vehicle using SBUS

## Requirements:

### Parts List:

* 1x UXV Micronav Dev Kit (or another controller with SBUS output)
* 1x UXV Airside Integrated Module (firmware > v2.0.0)
* 2x Doodle Labs radios with working connection
* 1x JHEMCU SBUS to PWM converter
* 1x 5V UBEC
* 1x VESC (open-source electronic speed controller)

### Consumables List

* Min 5x 2-Pin connectors of your choice, this guide uses XT60/XT90 (male and female)
* Min 3x Bare pin connectors, this guide uses 4mm bullet connector (male and female)
* Min 0.5m AWG10 cable
* Min 1m AWG16 cable
* Min 3x 3-Pin servo connection leads (both male and female)

### Tools List:

* Soldering Iron (recommended at least 100W)

### (Optional:)

* 4x 20dB attenuators for bench testing of Doodle Labs radios
* Bench power supply&#x20;

{% hint style="warning" %}
Note: This guide is building upon a TRAXXAS MAXX rc car, which already has a BLDC motor and battery.
{% endhint %}

## Connection Diagram

{% hint style="info" %}
The following diagram shows the connection on the UxV side.
{% endhint %}

<figure><img src="../.gitbook/assets/SBUS-Rc-Car-Connection (7).png" alt=""><figcaption></figcaption></figure>

## 1. Check Firmare

The Micronav controller needs to be flashed with special firmware, which helps smooth out the signal coming from the radio on the AirSide module. It also enables failsafe values on the airside module. To set them up, you should use the NavSuite on the Micronav, as will be explained later. For now, the important step is to ask UXV to use the special firmware on the MicroNav.

## 2. Explaining the connections

### A. Voltage Sag

A Li-Po battery is not an ideal power source. When components ramp up current draw from it, the voltage momentarily drops, which can for example cause an undervoltage error in the VESC when drawing a lot of power from the steering servo.&#x20;

Therefore it is recommended to use a BEC (battery eliminator circuit) to power the radios, unless they have their own (some models, such as the one UXV is using). This way you can make sure the radios get a constant voltage input. It also ensures that any back-current noise coming from the ESC (for example when breaking) will not disturb the transmitted signal.

### B. Powering Several Components

It is common to have several components drawing power connected to the battery. The components should each be connected in parallel. This is done by splitting both the (+) and (-) cable into the same amount of cables as there are components, in one point. The following schematic shows how to power 3 ESCs from one battery using the star topology, which is the optimal used one.&#x20;

<figure><img src="../.gitbook/assets/3ESC+1Battery-Connection.drawio (1).png" alt=""><figcaption></figcaption></figure>

### C. Ground Connections

All of the components need to be grounded. However not all grounds are the same. Some voltage regulators have a [floating](../#floating-power-rail) or [isolated](../#isolated-power-rail) power rails. They are good at protecting the components from overvoltages. On the other hand, it can be tricky if you are using a type of communication protocol where the receiving signal is read against the components ground reference (such as SBUS).

Connecting the two components with a&#x20;
