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

### 2 Flight Controller Configuration

{% hint style="info" %}
This step assumes you have already set up your PX4 vehicle controller according to [this guide](../gcs-uxv-and-antenna-setup/setting-up-a-px4-vehicle.md).
{% endhint %}

Completely disconnect the PX4 vehicle controller from the radios and only use an connection through USB to your computer. On you pc open QGroundControl and navigate t



