---
description: This guide explains techniques and workflows for debugging an SBUS connection.
---

# Troubleshooting SBUS Connection

This guide offers several workflows for troubleshooting the connection made in the previous guide. For the steps, it is reccomended to disconnect the UGV components and connect a simple servo, which will allow you to observe the signal changes better.

## 1. Check Baudrate Settings

A common pitfall is a baudrate mismatch in the settings. Verify all of the components keep the workflow:

<figure><img src="../.gitbook/assets/UART-to-PWM-Flowchart (1).png" alt=""><figcaption></figcaption></figure>

| Component      | Baudrate Output | Where to change |
| -------------- | --------------- | --------------- |
| Micronav       | 115 200         | Contact UXV     |
| GCS Radio      | 115 200         | Radio Web GUI   |
| UAV Radio      | 115 200         | Radio Web GUI   |
| Airside Module | 100 000         | Contact UXV     |

A common symptom of a baudrate mismatch is a glitchy servo. If the baudrates are mismatched within a margin, every once in a while the signal will match and produce the correct output moving the servo for example. The frequency of the servo working and not working should stay constant for this glitch.

## 2. Verify Signal Output from all Components

The next step to verify all of the components are outputting signal (are not dead), use an oscilloscope. The operating steps for your model can be different, however the basic principle is that you should connect the crocodile clip to a ground and touch the different pins in the setup with the probe. When the oscilloscope is set correctly, it will display a square wave for a working UART signal.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 153332.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If the signal is a constant low, there is no output and the line is not working. If the output is a constant high, the line is SBUS, however it is idle and no data is flowing.
{% endhint %}

{% hint style="info" %}
If the signal from the UXV MIcronav appears clean, but the signal from UXV Airside module appears glitchy (wave shifting, rough signal edges etc), check the grounding of components as a SBUS signal needs a good ground reference.
{% endhint %}

## 3. SBUS to PWM from lost packages

If you are using the UXV Airside module with the correct firmware for error compensation, this should not be a problem. Some SBUS to PWM converters, might however be thrown off-course by occasional errors in the frames and freeze the output until a new frame of the same logic is received.

## 4. Using a Logic Analyzer

If all of the cables are transmitting the signal correctly, but the connection is bad, you can use a logic analyzer.
