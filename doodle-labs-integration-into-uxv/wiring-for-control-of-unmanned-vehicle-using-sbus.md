---
description: >-
  This page explains how to wire a connection for an unmanned vehicle, which
  will be controlled using SBUS. It is a precourse for the following guide,
  which explains the configuration.
---

# Wiring for Control of Unmanned Vehicle using SBUS

## Requirements:

### Parts List:

* 1x UXV Micronav Dev Kit (or another controller with UART output)
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
The following diagram shows the connection on the UxV side. As per electronics convention, a GND (-) cable is black, a VCC (+) cable is red. Signal cables have various other colors.
{% endhint %}

<figure><img src="../.gitbook/assets/SBUS-Rc-Car-Connection (9).png" alt=""><figcaption></figcaption></figure>

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

Connecting the two components with a ground wire will make the connection work, but you will lose isolation of the sensitive component.

In order to make the connection work without ruining the isolation of the component, the signal needs to be transferred using either light or magnetic field. Some common components used for this purpose are octocouplers or digital isolators.

{% hint style="info" %}
If you know the number of outputs and the target voltage, it is common to use a PDB (power distribution board) to simplify the connections.
{% endhint %}

{% hint style="danger" %}
A well grounded connection is especially important for SBUS.
{% endhint %}

## 3. Making the Connections

### A. VESC PWM Connection

{% hint style="info" %}
The VESC UXV Technologies is using is a custom made board based on the open source VESC project. Everything that applies to the board will apply to every other VESC board. If you do not wish to use a VESC, any other ESC with a sufficiently high power rating will do the job.
{% endhint %}

Firstly we will swap out the original TRAXXAS ESC for a VESC. There are several reasons to do this:

* The maximum current the ESC can supply to servos and other electronics is 3A (TRAXXAS MAXX), which might not be sufficient to run other electronic.&#x20;
* The ESC only offers 3 control modes. It does not allow the user to control:
  * Ramp-up time
  * Throttle curve
  * Brake
  * Top speed
  * Failsafe

On the other hand, the TRAXXAS ESC is waterproof.

This setup uses a PWM signal to control the VESC. The connectors on the various VESCs will be different, for example the UXV one only has soldering holes, however the important thing is that the cable coming out of it is a male servo cable, no matter how you make the cable.

The following picture is an example of what the PWM input pins can look like.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 081317.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Expected outcome: Connected cable for PWM output, which has a male servo connector
{% endhint %}

{% hint style="warning" %}
The PWM pins on the VESCs are commonly labelled PPM, which is a historical naming mistake. They are in fact PWM pins.
{% endhint %}

### B. VESC-Motor Connection

The VESC produces a switching signal through 3 wires which drive the different phases of the motor. The cables do not have polarity and can be used interchangeably as the VESC will automatically detect the phase it is connected to.&#x20;

The most common connectors used are bullet connectors, while other people use MT60/MR60 connectors. For high performance applications where the ESC will not be changed, the cables between the motor and the ESC are soldered directly.

When using bullet connectors, it is common to use female on the ESC side and cover them with heat shrink tubes on the outside. This way the connectors will not conduct electricity if they touch on accident, which would likely fry the ESC.

The following picture is an example of a VESC connection:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 082308.png" alt=""><figcaption></figcaption></figure>

