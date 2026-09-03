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

### 1. Flash the Flight Controller with Firmware

Start by connecting the Flight Controller to your computer via USB and open Mission Planner.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 144100.png" alt=""><figcaption></figcaption></figure>

In Mission Planner, select the correct COM port in the top-right corner and click connect.&#x20;

{% hint style="info" %}
If you need to find out to which COM port the flight controller is connected to, open Device Manager on your Windows computer. Open the "Ports (COM & LPT)" drop-down. Now connect the flight controller and notice which port appears, which is the one the flight controller is connected to.

![](<../../.gitbook/assets/Screenshot 2026-09-03 144545 (1).png>)
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 145013.png" alt=""><figcaption></figcaption></figure>

Continue by navigating to "Setup" - "Install Firmware". A screen will appear stating that firmware cannot be loaded while connected via MavLink and asking the user to click on "Disconnect" in the top right corner. Click on it and a screen with possible versions appears.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 145432.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 145800.png" alt=""><figcaption></figcaption></figure>

Select the correct ArduPilot distribution for your use case.&#x20;

* [Rover](https://ardupilot.org/rover/) - Use for ground vehicles (cars, tracked vehicles) and surface watercraft.
* [Plane](https://ardupilot.org/plane/) - Use for Fixed-Wing (airplane, glider, flying wing) and Hybrid Aircraft (Quad-Plane VTOL)
* [Sub](https://ardupilot.org/sub/) - Use for underwater vehicles (ROV, UUV etc).
* [Antenna Tracker](https://ardupilot.org/antennatracker/) - Use for Ground Control Station (GCS) directional antenna hardware.
* [Various Copter Frames](https://ardupilot.org/copter/)
  * Quadcopter
  * Hexacopter
  * Helicopter
  * Octo-Quad (Coaxial Quad)
  * Tricopter
  * Y6 Coaxial
  * Octocopter

For this use-case, UXV Technologies will flash the Flight Controller with the Rover build. A window will open, prompting the user to select the correct Flight Controller platform. In this case, the basic Pixhawk 6X has been selected.

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 151105.png" alt=""><figcaption></figcaption></figure>

Continue by clicking "Upload Firmware". After the initial setup, a prompt will pop up asking to disconnect and reconnect the Flight Controller and hit the OK button within 30 seconds.

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 151221.png" alt=""><figcaption></figcaption></figure>

After the firmware has been uploaded, the COM port the Flight Controller is connected to will very likely change if you uploaded a different firmware. This time however, Mission Planner will recognise the port and mark it as MavLink. Select the port in the drop-down menu and click on "Connect".

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-03 151935.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The SLCAN option will turn the Flight Controller into a pass-through USB-to-CAN adapter, allowing you to talk directly to a peripheral device connected to it.&#x20;
{% endhint %}

### 2 Connect Peripherals

For further setup, it is best to connect the peripherals you wish to use on the final build to directly test the settings. In this guide UXV Technologies used:

* 1x Holybro GPS connected to GPS\&SAFETY
* 1x VESC controlled over CAN 2
* Servo connected through the servo rail
* 5V voltage step-down module to power the servo
* 1x CAN PMU (or equivalent power module for the flight controller) if you are using an ESC which does not send power information

{% hint style="info" %}
Connecting the GPS to the GPS\&SAFETY port enables the following

* Physical safety switch to prevent accidental arming (additional layer of safety)
* LED Pin for light status signaling (blinking = safe, solid = armed)
* Buzzer Pin for sound status signaling
{% endhint %}

#### 1. GPS Connection

It is reccomended to use the GPS\&SAFETY port on the flight controller to enable all of the following functions:

* Buzzer Pin for sound status signaling
* LED Pin for light status signaling (blinking = safe, solid = armed)
* Physical safety switch to prevent accidental arming (additional layer of safety)

&#x20;Other port options (for various GNSS models):

* TELEM
* UART
* CAN
* GPS
