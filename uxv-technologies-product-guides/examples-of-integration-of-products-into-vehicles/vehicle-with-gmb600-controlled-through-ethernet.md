# Vehicle with GMB600 Controlled through Ethernet

{% hint style="info" %}
This guide explains the process of integrating a the Optroxa GMB600 into an unmanned vehicle with other UXV Technologies products using the ethernet connection.
{% endhint %}

### Part List:

* 1x Optroxa GMB600 Gimbal
* 2x SRM Radios
* 1x SRM Airside ETH (or other equivalent SRM Airside module that will act as a switch)
* 1x SRoC Controller
* 1x Vehicle with a flight controller

### Cable List:

* 1x ODU A10WAM-P12XMM0-0000 to ODU G81 - 8 pin and Power (such as XT30) for connecting GMB600 to SRM Airside (UXV does not make one yet, coming soon)

{% hint style="info" %}
Prerequisites:

1. Flight Controller with [PX4](../../vehicle-and-ground-station-wiring-and-setup/flight-controller-firmware/px4/setting-up-a-px4-vehicle.md) or [ArduPilot](../../vehicle-and-ground-station-wiring-and-setup/flight-controller-firmware/ardupilot/setting-up-an-ardupilot-vehicle.md) set-up
2. Other vehicle peripherals connected based on [this guide](../../vehicle-and-ground-station-wiring-and-setup/peripherals/physical-wiring-of-peripherals/)
3. SRM Airside ETH is set up according to [this guide](../radios/srm-airside-eth-setup.md).
{% endhint %}

### 1. Explaining the architecture

<figure><img src="../../.gitbook/assets/GMB600_SRM-Airside_FC_SRM-DL_Connection-Diagram (5).png" alt=""><figcaption></figcaption></figure>

This architecture, where the gimbal and flight controller are on the same network, is typical for an unmanned vehicle. The SRM Airside ETH acts as a switch, bringing both of the devices and the radio on the same network. This is advantageous for two reasons:

1. The video and telemetry stream are unified on one link, eliminating any redundancies of old systems where the streams were split.
2. The gimbal has a NVidia Jetson Orin Nano integrated in it turning it into an edge-compute node. By having the flight controller on the same network as the Jetson, [autonomy](../../explained-concepts/basic-autonomy-using-ros2.md) is enabled in the vehicle without having to strain the radio link by sending the data to the GCS for processing or adding an extra computing note, which would consume more power and increase the wiring complexity.

{% hint style="info" %}
The UXV Technologies gimbal also has CAN pins exposed to allow the gimbal to control other devices and UART for&#x20;
{% endhint %}

Below is the wiring diagram of the complete vehicle that UXV Technologies used for testing.

<figure><img src="../../.gitbook/assets/Shcematic-Diagram-GMB600-Vehicle (3).png" alt=""><figcaption></figcaption></figure>

### 2. Stream Data to Custom QGC+

Finish this section based on information from Andy.

## (3.) Quick-Start Guide

### Component List:

* Vehicle based on the setup above
* 2x SRM
* UXV Technologies Controller with SRM slot (or other connection)

### (3.1) Step 1: Turn on the Ground Control Station and SRM

Begin by powering on the controller and the SRM radios. For most of them, changing the position of the safety switch is enough, however for the L-designated radios, connect an external battery.

{% hint style="info" %}
It is also possible to use a computer with the SRM and a controller connected externally, such as with a G2Nav, DualGrip and SRM MOLLE Dock.
{% endhint %}

Then power on the vehicle and wait for all of the components to boot up, which takes about 2 minutes.

### (3.2) Step 2: Connect to the Flight Controller

Launch QGroundControl (gimball support has not been added to Mission Planner yet) and click on "Disconnnected - Click to Manually Connect".

<figure><img src="../../.gitbook/assets/Screenshot 2026-09-08 171304.png" alt=""><figcaption></figcaption></figure>

Continue by clicking on the small arrow and then on "Configure".

{% hint style="info" %}
To verify the connection has been established and is working, navigate to Windows Control Panel - Network and Internet - Network and Sharing Center - Change Adapter Settings - click on the adapter you expect to use - Details.&#x20;

There you will see the IP adress of the adapter, which should be on the 10.224.1.1/16 subnet (or the one you set the SRM Airside ETH to assign to DHCP clients). If it is 169.254.1.1/16, is an adress Windows defaults to if it cannot connect to a DHCP server.
{% endhint %}

The gimbal live view should appear automatically in QGC.
