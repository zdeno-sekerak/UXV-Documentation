---
description: >-
  This guide explains how to integrate the SRM Airside ETH into your UxV
  (Unmanned Vehicle).
---

# SRM Airside ETH Setup

<figure><img src="../../.gitbook/assets/image__5_-removebg-preview.png" alt=""><figcaption></figcaption></figure>

## Part list:

* SRM Airside ETH
* 2x SRM Radio (This guide uses SRM-S-DL+)
* 1x ODU S11YAR 8 Pin to 4-pin JST-GH1.25 with ETH pinout
* 1x ODU S11YAR 8 Pin to whatever the GMB600 connector is
* 1x ODU S10YAR 10 Pin to XT60 (or other power connector)
* 1x UXV SRoC

Optional Components/Tools:

* SRM Dock with Xt30 and USB-A - Used for configuration/debugging of the SRM

## 1. What is the SRM Airside ETH

The SRM Airside is a linux computer running OpenWRT. This way it can act as a switch for the radio connections.

It has the following physical ports:

* 2x Ethernet on the ODU G81 - 8 pin
* 1x Ethernet in the SRM slot
* 1x Power and UART on the ODU G80 - 10 pin

By default, the first ethernet port on the side is set as a configuration port to directly access the SRM Airside ETH. The IP is the default IP set by OpenWRT - 192.168.1.1. The other ethernet port is a passthrough port bridged to the SRM Radio.

## 2. Connecting to the SRM Airside ETH

