---
description: This guide explains how to integrate the SRM Airside ETH into your UxV.
---

# SRM Airside ETH Setup

## Part list:

* SRM Airside ETH
* 2x SRM Radio (This guide uses SRM-S-DL+)
* 1x ODU S11YAR 8 Pin to 4-pin JST-GH1.25 with ETH pinout
* 1x ODU S11YAR 8 Pin to whatever the GMB600 connector is
* 1x ODU S10YAR 10 Pin to XT60 (or other power connector)
* 1x UXV SRoC

Optional Components/Tools:

* SRM Dock with Xt30 and USB-A - Used for configuration/debugging of the SRM

## 1. Connecting to the SRM Airside

The SRM Airside is a linux computer running OpenWRT. It has 3 physical ethernet ports (2 on the side and 1 for the SRM) and one power connector, which also has UART for debugging.&#x20;
