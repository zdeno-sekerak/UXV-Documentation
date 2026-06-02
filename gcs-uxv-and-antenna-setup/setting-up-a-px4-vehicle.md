---
description: This guide explains how to set up an out-of-the-box flight controller.
---

# Setting up a PX4 Vehicle

Part List:

* PX4-Compatible Flight Controller

Tools:

* Computer/UXV Controller with Windows
* USB Cable - To connect the flight controller to the computer/UXV controller

### 1 What is PX4?

### 2 Install QGC

Begin by installing [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html) on your SRoC. This is a software used to set up vehicle controllers, get data from them during operation and in our case also to control the vehicle itself. Optionally you can also install the software on your computer as using a mouse and a keyboard will make configuring everything easier.

Continue by verifying that the flight controller has the correct firmware installed. In this guide we will be using the PX4, which is an open-source vehicle controller software. Open QgroundControl on your computer or SRoC and connect the vehicle controller via an USB cable. QGroundControl should automatically detect it and load its parameters.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption><p>An Example of a USB Connection to a Flight Controller on the Cube Orange+</p></figcaption></figure>

If you have a new flight controller, it probably ships without firmware and needs to be flashed with PX4. To start the flashing process, disconnect the flight controller from your computer and make sure it is not powered by a battery for example. Continue by clicking on the Q logo in the top left corner in QGroundControl, selecting vehicle configuration and going to firmware.

<figure><img src="../.gitbook/assets/unknown (14).png" alt=""><figcaption></figcaption></figure>

Then connect a vehicle controller, PX4 will detect it and install the default version of PX4.

The next step is to select the airframe you will be using in Vehicle Configuration - Airframe. After selecting the correct one hit Apply and restart in the top right corner.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
If you wish to use a different type of vehicle than the ones listed, for example a car, you will need to build your own version of PX4 software as described in [this guide](https://docs.px4.io/main/en/dev_setup/building_px4).
{% endhint %}

Continue by mounting the flight controller on the vehicle of tour choice. Make sure to mount it on a vibration-damping surface and as close to the center of mass of the vehicle as possible. Then connect all of the electronics and ideally power on the battery.

Once the vehicle is powered on, you can start callibrating the sensors in Vehicle Configuration - Sensors.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

It is important to calibrate the sensors in the airframe as some of them will be disturbed by power cables and other metal parts, which the controller needs to detect and compensate for.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>Electronics Inside of the Skywalker X8 Flying Wing</p></figcaption></figure>

Then you can start calibrating the [Servos and the ESC](setting-up-actuators-servos-and-motors-in-px4.md).
