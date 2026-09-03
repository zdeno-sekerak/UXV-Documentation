---
description: This page explains the process of setting up an ArduPilot vehicle.
---

# Setting up an ArduPilot Vehicle

### Part List:

* Flight Controller (this guide uses CUAV V6X)
* GNSS (this guide used Here4)
* Servo (optional amount)
* 1x CAN PMU (or any other unit to power the flight controller)
* 5V UBEC (amount depends on number of components)
* 6V UBEC (amount depends on number of components)

### Optional Part List:

* VESC (optional amount - keep in mind the required 120Ω resistance requirement) or any other ESC
* BLDC Motor (most common type)

### Tool List

* Windows Computer with Mission Planner installed (this guide uses Windows 11)

### 1. Flash the Flight Controller

Start by connecting the Flight Controller to your computer via USB and open Mission Planner.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-09-03 144100.png" alt=""><figcaption></figcaption></figure>

In Mission Planner, select the correct COM port in the top-right corner and click connect.&#x20;

{% hint style="info" %}
If you need to find out to which COM port the flight controller is connected to, open Device Manager on your Windows computer. Open the "Ports (COM & LPT)" drop-down. Now connect the flight controller and notice which port appears, which is the one the flight controller is connected to.

![](<../.gitbook/assets/Screenshot 2026-09-03 144545 (1).png>)
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2026-09-03 145013.png" alt=""><figcaption></figcaption></figure>

Continue by navigating to "Setup" - "Install Firmware". A screen will appear stating that firmware cannot be loaded while connected via MavLink and asking the user to click on "Disconnect" in the top right corner.

<figure><img src="../.gitbook/assets/Screenshot 2026-09-03 145432.png" alt=""><figcaption></figcaption></figure>

