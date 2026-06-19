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

{% hint style="info" %}
It is possible to measure the baudrate of a connection on the oscilloscope, however identifying the frames correctly can be tricky and therefore it is reccomended to use a logic analyzer. If you decide to use an oscilloscope anyways, make sure to double-check measurements.
{% endhint %}

## 3. Reading the Values from UXV Airside

The UXV Airside module has a function which allows you to debug its working. To connect to it, use a cable from USB-C port on the airside to your computer. Then use the application termite, or other serial monitor.

Start by opening Termite and clicking on Settings:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 073934.png" alt=""><figcaption></figcaption></figure>

Then set the values according to the following, except the COM port, which will be different for every computer.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 074015.png" alt=""><figcaption></figcaption></figure>

To find the correct port, open Device Manager on windows, open the Ports (COM & LPT) drop-down, then connect the UBS from UXV Airside and observe which port appears. In the case of the UXV setup, it is the COM11 which we will connect to in Termite.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 074134.png" alt=""><figcaption></figcaption></figure>

Next click ok in the port settings in Termite and connect to the port. The Airside module will periodically start sending messages.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 074041.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 074246.png" alt=""><figcaption></figcaption></figure>

Next, type in the command "start\_stream 2". The Airside will start printing live values with 1s update time, which can help verify the input/output is correct.

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 165735.png" alt=""><figcaption></figcaption></figure>

## 4. SBUS to PWM from lost packages

If you are using the UXV Airside module with the correct firmware for error compensation, this should not be a problem. Some SBUS to PWM converters, might however be thrown off-course by occasional errors in the frames and freeze the output until a new frame of the same logic is received.

## 5. Using a Logic Analyzer

If all of the cables are transmitting the signal correctly, but the connection is bad, you can use a logic analyzer.

The setup you should use will depend on the logic analyzer. UXV used Analog Discovery 3 pro from Digilent paired with the Waveforms software.&#x20;

Begin by connecting the logic analyzer and setting the expected baudrate and signal type in the software. From the read signal, you can see if the signal from the airside is:

* Readable
* Correct wavelengt
* Correct baudrate
* Correct logic (SBUS is inverted)

it is recommended to read the data from the SBUS output of the airside as SBUS uses a standardized logic for encoding channel values for joysticks and switches.

{% hint style="danger" %}
To read the baud rate from the logic analyzer correctly, make sure to understand the logic of the protocol you are using. For example the length of an SBUS frame in the picture is $$115.2 ms$$ as the frame has 11 bits in it. If one bit is measured, it would give the correct value for the SBUS frame.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2026-06-19 162028.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Analyzing the SBUS to PWM converter is difficult as the JHEMCU one is not open source. If having problems with it, try contacting the manufacturer to analyze the behavior as having a look at the code the converter is running can make the process  of understanding what is happening internally significantly simpler.
{% endhint %}
