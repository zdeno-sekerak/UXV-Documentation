---
description: >-
  This page explains how to connect to a PX4 vehicle from a windows computer
  through ethernet by using UXV SRM Radios.
---

# Ethernet Setup for PX4

{% hint style="info" %}
For the most up-to-date guide, please check out official [PX4 documentation](https://docs.px4.io/main/en/advanced_config/ethernet_setup).
{% endhint %}

{% hint style="info" %}
This guide uses the following:

* SRoC 7" with Windows 11
* CUAV V6X Flight Controller
* QGC v5.0.8
* PX4 v1.16
{% endhint %}

### 1. Settinga static IP Adress of the Controller

{% hint style="info" %}
This step assumes you have already set up your PX4 vehicle controller according to this guide.&#x20;
{% endhint %}

Completely disconnect the PX4 vehicle controller from the radios and only use an connection through USB to your computer. Open QGC on your computer.

To make the configuration work on the current setup, go to QGC Icon -> Analyze Tools -> MavLink Console. The configuration file is located in /fs/microsd/net.cfg on the SD card and the file contains text where each of the settings is written on a new line as a name=value pair. Type in the following in the console to change values in the file:&#x20;

```c
echo DEVICE=eth0 > /fs/microsd/net.cfg 
echo BOOTPROTO=static >> /fs/microsd/net.cfg
echo IPADDR=10.223.218.99 >> /fs/microsd/net.cfg
echo NETMASK=255.255.255.0 >> /fs/microsd/net.cfg
echo ROUTER=10.223.218.204 >> /fs/microsd/net.cfg
echo DNS=10.223.218.204 >> /fs/microsd/net.cfg 
```

Next, type the command: netman update. Be careful not to use the command netman save as it will load the previous configuration from the volatile memory and override the one you had just set up.

{% hint style="info" %}
If you wish for the flight controller to get the IP address from a DHCP server, set:\
echo BOOTPROTO=dhcp >> /fs/microsd/net.cfg
{% endhint %}

The IPADDR setting changes the IP adress of the Vehicle Controller. UXV is using 10.223.218.99, however you can use any other one as long as it is not the same as any of the radios. If you are using a different one, make sure to note it down as it will be useful later. The ROUTER and DNS settings state the IP of the UxV radio, so it is going to be different for each setup. BOOTPROTO is set to statis, which means the Vehicle Controller will have the IP defined by IPADDR.&#x20;

To check that your settings are correct, completely disconenct the flight controller and wait for a few seconds. Then recconect it, open MavLink console and type:

```bash
netman show
```

The printed settings should be the same as you had just set up. If the wrong commands are used, the settings will only be stored in volatile memory. After being power cycled, the volatile memory is cleared and the settings are loaded from the net.cfg file on the SD card.

{% hint style="success" %}
Expected outcome: The correct network settings are stored on the SD card.
{% endhint %}

### 2. Configuring the Ethernet port

To configure the ethernet port on the Vehicle, navigate to QGC Icon -> Vehicle Setup -> Parameters and set the following:&#x20;

| Parameter                                                                                               | Value                    | Explanation                                                                                   |
| ------------------------------------------------------------------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------------------- |
| [MAV\_2\_CONFIG](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_CONFIG)          | 1000 (Ethernet)          | Configures the port as Ethernet                                                               |
| [MAV\_2\_BROADCAST](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_BROADCAST)    | 1 (Always Broadcast)     | Broadcasts Heartbeat Messages, lets the network know it is alive                              |
| [MAV\_2\_MODE](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_MODE)              | 0 (Normal)               | Treat whatever is plugged to this port as a GCS (through a network)                           |
| [MAV\_2\_RADIO\_CTL](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_RADIO_CTL)   | 0 (Disabled)             | Disables the drones ability to slow down or compress sent data if the network cannot keep up. |
| [MAV\_2\_RATE](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_RATE)              | 100000 (Bits per second) | Maximum sending rate                                                                          |
| [MAV\_2\_REMOTE\_PRT](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_REMOTE_PRT) | 14550                    | Makes the Vehicle Controller listen at port 14550                                             |
| [MAV\_2\_UDP\_PRT](https://docs.px4.io/main/en/advanced_config/parameter_reference#MAV_2_UDP_PRT)       | 14550                    | Makes the vehicle controller send data to port 14550 on the GCS                               |

For parameter reference visit this [link](https://docs.px4.io/main/en/advanced_config/ethernet_setup#px4-mavlink-serial-port-configuration).

To upload the parameter settings, reboot the vehicle controller, ideally by completely disconnecting and reconnecting it.

{% hint style="success" %}
Expected outcome: The vehicle controller ethernet port is configured correctly.
{% endhint %}

### 2.3 Setting up QGroundControl

In the previous steps, we had been guided on how to change the settings on the Vehicle Controller by using QGroundControl to write the data. Now the QGroundControl settings themselves need to be configured to read the correct data from UXV controller and the Vehicle Controller.

Begin by connecting the whole setup and powering it on.&#x20;

An example of a simple point-to-point setup: UXV Controller -> GCS Radio -> UAV Radio -> Vehicle Controller

In QGroundControl Click on 'Disconnected - Click to manually connect' in the top left corner.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-06-03 140042.png" alt="" width="375"><figcaption></figcaption></figure>

Proceed by clicking on the little arrow in the top right corner of the pop-up.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-06-03 140623.png" alt="" width="375"><figcaption></figcaption></figure>

Then click on 'configure' communication links.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-06-03 140755.png" alt="" width="375"><figcaption></figcaption></figure>

And click on 'Add Link.'

<figure><img src="../../../.gitbook/assets/Screenshot 2026-06-03 140849.png" alt="" width="375"><figcaption></figcaption></figure>

Then set up according to the following:

<figure><img src="../../../.gitbook/assets/Screenshot 2026-06-03 140605.png" alt="" width="281"><figcaption></figcaption></figure>

By setting the type as UDP, the data transmission will be faster, but there will be less integrity checks in the conection itself. On the other hand, that does not matter much as PX4 does have its own.

The port number is the same as when setting up the ethernet port in QGC. UXV also noticed the establishing of the connection seems to be quicker when the server adress is set. The format is:&#x20;

```
<IP adress of the vehicle controller>:<Port number>
```

{% hint style="success" %}
Expected Outcome: Flight controller is connected through ethernet to a computer.
{% endhint %}

For controlling an Unmanned Vehicle through ethernet, follow the next guide.
