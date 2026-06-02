---
description: >-
  This guide explains how to set up an ethernet with Doodle Labs radios to
  control a PX4 Unmanned Vehicle with an SRoC.
---

# Controlling an Unmanned Vehicle with Ethernet and QGC

{% hint style="info" %}
This guide assumes you have a working communication link between the radios and have a basic understanding of how it works.
{% endhint %}

{% hint style="warning" %}
This guide only focuses on UXV SRoC controllers running Windows operating system, however the setup for Linux and Android and other controllers should be similar.
{% endhint %}

Part list:

* 2x Doodle Labs Radio - Working and optimized link
* 1x Vehicle Controller with ethernet port and PX4 installed - Using CUAV V6X
* 1x UXV Controller - Using SRoC 7'' Windows
* 1x Ethernet Adapter for the Controller - Using SRM-RJ45

### 1 Create Connecting Cables

#### 1.1 GCS Radio + GCS Controller

Start by connecting the radio which has been configured as a GCS to a controller through ethernet. In a previous guide, we showed how to wire a RJ-45 connector to the ethernet pins on the Doodle Labs radio. Now use the connector to connect the radio to SRM-RJ45 adapter on the SRoC Controller.&#x20;

Alternatively, if you are still in the testing phase, you can use an evaluation board to simplify the connection.

An example of such a connection is shown below:

<figure><img src="../.gitbook/assets/Screenshot 2026-06-02 131225.png" alt=""><figcaption></figcaption></figure>

Several alternatives are also shown below with the UXV Micronav Dev Kit, which has a port to connect a doodle labs radio directly. In the second picture, UXV used a simple ETH to USB-A adapter to substitute for the RJ-45 adapter. The point of this step is to have a working connection between the controller and the radio.

{% columns %}
{% column valign="middle" %}
<figure><img src="../.gitbook/assets/Screenshot 2026-06-02 131032.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column valign="middle" %}
<figure><img src="../.gitbook/assets/Screenshot 2026-06-02 131008.png" alt=""><figcaption></figcaption></figure>


{% endcolumn %}
{% endcolumns %}

The reccomended wiring for a GCS however remains a simple DL Radio -> SRM-RJ45 -> SRoC.

#### 1.2 UxV Radio + PX4 Vehicle Controller

Continue by wiring the ethernet connection from the UAV radio to the flight controller as explained in the [guide](../doodle-labs-connection-setup/physical-connection-without-an-evaluation-board.md).&#x20;

2 Flight Controller Configuration
2.1 Setting IP Adress of the Controller
2.1 Setting IP Address of the Controller
This step assumes you have already set up your PX4 vehicle controller according to this guide.
Completely disconnect the PX4 vehicle controller from the radios and only use an connection through USB to your computer.
Completely disconnect the PX4 vehicle controller from the radios and only use an connection through USB to your computer. On you pc open QGroundControl and navigate t
Completely disconnect the PX4 vehicle controller from the radios and only use an connection through USB to your computer.
INSERT AN IMAGE OF A FLIGHT CONRTOLLER CONNECTED WITH A CABLE
To make the configuration work on the current setup, go to QGC Icon -> Analyze Tools -> MavLink Console. The configuration file is located in /fs/microsd/net.cfg on the SD card and the file contains text where each of the settings is written on a new line as a name=value pair. Type in the following in the console to change values in the file:
echo DEVICE=eth0 > /fs/microsd/net.cfg
echo BOOTPROTO=static > /fs/microsd/net.cfg
echo IPADDR=10.223.218.99 > /fs/microsd/net.cfg
echo NETMASK=255.255.255.0 > /fs/microsd/net.cfg
echo ROUTER=10.223.218.204 > /fs/microsd/net.cfg
echo DNS=10.223.218.204 > /fs/microsd/net.cfg
Next, type the command: netman update Be careful not to use the command netman save as it will load the previous configuration from the volatile memory and override the one you had just set up.

The IPADDR setting changes the IP adress of the Vehicle Controller. UXV is using 10.223.218.99, however you can use any other one as long as it is not the same as any of the radios. If you are using a different one, make sure to note it down as it will be useful later. The ROUTER and DNS settings state the IP of the UxV radio, so it is going to be different for each setup. BOOTPROTO is set to statis, which means the Vehicle Controller will have the IP defined by IPADDR.
The above settings gave the vehicle controller an IP adress on the ethernet network.
2.2 Configuring the Ethernet port
To configure the ethernet port on the Vehicle, navigate to QGC Icon -> Vehicle Setup -> Parameters and set the following:
MAV_2_CONFIG 1000 (Ethernet) MAV_2_BROADCAST 1 (Always Boradcast) MAV_2_MODE 0 (Normal) MAV_2_RADIO_CTL 0 (Disabled) MAV_2_RATE 100000 (100000 Bits per second) MAV_2_REMOTE_PRT 14550 (Sets 14550 as the remote port number, which is the number at which PX4 listens to GCS Messages) MAV_2_UDP_PRT 14550 (Sets 14550 as the local port number)
For parameter reference visit this link https://docs.px4.io/main/en/advanced_config/ethernet_setup#px4-mavlink-serial-port-configuration.
This will set the framework for how the communication on the port works.
2.3 Configuring QGroundControl

After setting up everything
To make the configuration work on the current setup, go to QGC Icon -> Analyze Tools -> MavLink Console. The configuration file is located in /fs/microsd/net.cfg on the SD card. The file contains text where each of the settings is written on a new line as a name=value pair. Type in the following to change values in the file:
echo DEVICE=eth0 > /fs/microsd/net.cfg
echo BOOTPROTO=static > /fs/microsd/net.cfg
echo IPADDR=10.223.218.99 > /fs/microsd/net.cfg
echo NETMASK=255.255.255.0 > /fs/microsd/net.cfg
echo ROUTER=10.223.218.204 > /fs/microsd/net.cfg
echo DNS=10.223.218.204 > /fs/microsd/net.cfg
Next, type the command: netman update Be careful not to use the command netman save as it will load the previous configuration from the volatile memory and override the one you had just set up.
The IPADDR setting changes the IP adress of the Vehicle Controller. UXV is using 10.223.218.99, however you can use any other one as long as it is not the same as any of the radios. If you are using a different one, make sure to note it down as it will be useful later. The ROUTER and DNS settings state the IP of the UxV radio, so it is going to be different for each setup. BOOTPROTO is set to statis, which means the Vehicle Controller will have the IP defined by IPADDR.
The above settings gave the vehicle controller an IP adress on the ethernet network.
2.2 Configuring the Ethernet port
To configure the ethernet port on the Vehicle, navigate to QGC Icon -> Vehicle Setup -> Parameters and set the following:
MAV_2_CONFIG 1000 (Ethernet) MAV_2_BROADCAST 1 (Always Boradcast) MAV_2_MODE 0 (Normal) MAV_2_RADIO_CTL 0 (Disabled) MAV_2_RATE 100000 (100000 Bits per second) MAV_2_REMOTE_PRT 14550 (Sets 14550 as the remote port number, which is the number at which PX4 listens to GCS Messages) MAV_2_UDP_PRT 14550 (Sets 14550 as the local port number)
For parameter reference visit this link https://docs.px4.io/main/en/advanced_config/ethernet_setup#px4-mavlink-serial-port-configuration.
This will set the framework for how the communication on the port works.
2.3 Configuring QGroundControl
After setting up everything
