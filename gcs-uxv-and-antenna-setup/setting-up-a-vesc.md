---
description: This guide explains the basics of connecting and controlling a VESC.
---

# Setting up a VESC

A VESC is an open-source ESC which allows a significant level of function customization and is thus a great choice for custom builds. It is however a not a plug-and-play system and requires at least a basic understanding of how motor control works.

{% hint style="info" %}
Some of the VESC boards need to be flashed with firmware before use.
{% endhint %}

## 1. VESC-Motor Connection

The can be used to control BLDC (brushless DC), PMSM (pernament magnet synchronous motor) and a brushed DC motor. This guide focuses on controlling BLDC and PMSM motors, which are both 3-phase motors and both have 3 cables connected to an electronic speed controller. They however differ in wiring, which a VESC is able to detect.

Begin by soldering cables of correct thiccnes to the motor phase pads highlighted in the picture:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 144603.png" alt=""><figcaption></figcaption></figure>

A finished soldered cables can be covered with heat shrink tubing to avoid touching exposed cables while bench testing or integration.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 144843.png" alt=""><figcaption></figcaption></figure>

These cables can either be connected directly to the motor, or using connectors, which are the more practical option for testing as they allow the motor to be disconnected without powering off the VESC.

UXV used 4mm bullet connectors between the VESC and the motor, with the ESC side ones being female as they can be completely covered in heat shrink tubing meaning if they sit idle and accidentally touch, they will not short-circuit the VESC and burn it.

{% hint style="info" %}
It is reccomended to be careful when touching the VESC as static electricity (up to 25 000V) can accumulate on a human body when walking. It is recommended to only touch the board on the thin sides where no components are connected.
{% endhint %}

For integration, you can use a heat shrink tube or electrical tape to cover the PCB and only make cutouts for connectors to avoid damaging the board with static electricity, or accidentally short-circuiting the components with occasional exposed cables.



<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 145605.png" alt=""><figcaption></figcaption></figure>

A finished connection will look as following:

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 150339.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 150440.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

## 2. Connecting a VESC to a PC

As VESC is an open-source platform, the ESCs come in all shapes and sizes, this guide will therefore focus on a general procedure with the VESC made by UXV as a general example.

### 2.1 Power the VESC

To turn on the VESC, both the power (usually XT60) and connection cable to PC (USB) need to be connected. The input voltage range for the VESC is generally very wide, with the one UXV is using being 8V - 50V. After the power is turned on, the controller should light up.

Connected power cable on the VESC:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 141155.png" alt=""><figcaption></figcaption></figure>

Connected power and USB on the VESC:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 141129.png" alt=""><figcaption></figcaption></figure>

### 2.2 VESC Tool

The software, which is used to change the VESC settings is called [VESC Tool](http://vesc-project.com/vesc_tool) and can be downloaded from the VESC project website. This guide is using the version 6.06 of the software.

After launching the software with a connected VESC, the tool should recognize a connected VESC and will connect to it after clicking the "connect" button. Alternatively, it is also possible to click the "Autoconnect button."

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 141756.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 142119.png" alt=""><figcaption></figcaption></figure>

