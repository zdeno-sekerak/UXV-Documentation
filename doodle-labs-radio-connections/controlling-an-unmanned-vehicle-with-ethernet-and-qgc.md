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

### 1 Connecting the Flight Controller

#### 1.1 GCS and UxV radio

This guide assumes you have a working peer-to-peer (P2P) connection between two radios based on previous guides. The first step is to choose one of the radios to be a GCS and the other a UxV radio.

Continue by wiring the ethernet connection from the UAV radio to the flight controller and from GCS radio to the ethernet adapter on the controller based on th
