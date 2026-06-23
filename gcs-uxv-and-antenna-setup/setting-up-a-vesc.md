---
description: This guide explains the basics of connecting and controlling a VESC.
---

# Setting up a VESC

A VESC is an open-source ESC which allows a significant level of function customization and is thus a great choice for custom builds. It is however a not a plug-and-play system and requires at least a basic understanding of how motor control works.

{% hint style="info" %}
Some of the VESC boards need to be flashed with firmware before use.
{% endhint %}

{% hint style="info" %}
For additional resources, please refer to the [Vedder ESC documentation](https://vedder.se/2015/01/vesc-open-source-esc/).
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

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 151319.png" alt=""><figcaption></figcaption></figure>

Connected power and USB on the VESC:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 151303.png" alt=""><figcaption></figcaption></figure>

### 2.2 VESC Tool

The software, which is used to change the VESC settings is called [VESC Tool](http://vesc-project.com/vesc_tool) and can be downloaded from the VESC project website. This guide is using the version 6.06 of the software.

After launching the software with a connected VESC, the tool should recognize a connected VESC and will connect to it after clicking the "connect" button. Alternatively, it is also possible to click the "Autoconnect button."

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 141756.png" alt=""><figcaption></figcaption></figure>

## 3. Configuring the VESC

{% hint style="info" %}
This guide does not focus on setting up a VESC for a motor with a sensor.
{% endhint %}

## 3.1 Set Initial Values

Before running the callibration of the motor, change the following values according to your motor:

* ω - Target detection RPM. The speed the motor will spin up to during callibration.
* D - Duty cycle using detection. Sets the taget maximum average voltage on a scale from 0 to 1.
* I - Current during detection.

These values will then be used for callibration of the VESC in the next step.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 162929.png" alt=""><figcaption></figcaption></figure>

### 3.1 Setup Motor FOC

Begin by setting up the motor FOC by clicking the "Setup Motor FOC" button. [FOC](../#foc-field-oriented-control) is a control algorithm for 3-phase motors. Make sure to have the motor connected to the VESC and disconnect any wheels/propellers. The callibration will spin up the motor.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 142119.png" alt=""><figcaption></figcaption></figure>

Once you have entered the values for the motor and the battery and the callibration has been completed, write the motor and app configuration to the VESC to apply everything by clicking the highlighted buttons:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 161547.png" alt=""><figcaption></figcaption></figure>

### 3.2 Max Current and Voltage

Go into Motor Settings - General - Current/Voltage to set the maximum current and voltage your battery can provide.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 162355 (1).png" alt=""><figcaption></figcaption></figure>

You can also set the maximum BMS (battery management system), RPM and wattage in the top ribbon if you wish.

After you had set the values, write the configuration using the "Write Motor Configuration" and "Write App Configuration" buttons.

### 3.3 Observer

{% hint style="info" %}
The previous setup shoul produce a "good enough" result while driving a motor, which will be better then any cheap ESC. If you however want more from your ESC, you can set up an observer and many more complicated things beyond the scope of this guide.
{% endhint %}

The observer is a software which estimates the rotor position and speed in real time without any sensors present on the motor. To set the observer, go into Motor Settings - FOC - Advanced - Observer Type.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-22 165320.png" alt=""><figcaption></figcaption></figure>

To determine the correct observer, you will usually just need to try all of them and see which one works best.

It is also reccomended to set the "Control Sample Mode" to V0 and V7 Interpolation for a higher frequency of readings of the motor state.

### 3.4 Analyzing the VESC

To find out which observer works the best, enable realtime data by clicking the "RT" icon and keyboard control by clicking the keyboard icon on the right sidebar. This way the VESC Tool will read realtime data from the controller and you can also ramp up the throttle using left and right arrow keys.

The go into Data Analysis - Sampled Data, spin up the motor using the keyboard for a few seconds and in the meantime, click on the "Sample Now" button.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-23 084528.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The picture is an improperly callibrated VESC, the sampled data should absolutely not look like this.
{% endhint %}

The goal here is to get the "PH1", "PH2" and "PH3" to resemble a sinusoidal as close as possible and the "MC Total" to be a flat line.

As the motor is currently not under load for bench testing, the back-current is lower and the motor is much more sensitive to changes, therefore a result like the following is reasonable. The cyan line is the Motor Controller total current and the green one is the graph for the current of phase 1:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-23 091624.png" alt=""><figcaption></figcaption></figure>

With the load connected, the graph should look something like the following, where the phases follow a sinusoidal curve perfectly:

<figure><img src="../.gitbook/assets/Screenshot 2026-03-23 121140.png" alt=""><figcaption></figcaption></figure>

## 4 Additional Resources

The process of tuning a VESC to perfection is much more complicated then this guide covers and the result you will get with this guide is more "Good Enough". Therefore we have attached further resources to base your research on:

