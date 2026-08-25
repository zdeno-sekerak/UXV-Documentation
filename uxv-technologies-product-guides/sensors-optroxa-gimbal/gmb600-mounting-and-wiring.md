# GMB600 Mounting and Wiring

{% hint style="info" %}
This guide explains the process of integrating a the Optroxa GMB600 into an unmanned vehicle using other UXV Technologies products.
{% endhint %}

### Part List:

* 1x Optroxa GMB600 Gimbal
* 2x SRM Radios
* 1x SRM Airside ETH (or other equivalent SRM Airside module that will act as a switch)
* 1x SRoC Controller
* 1x Vehicle with a flight controller

### Cable List:

* 1x ODU A10WAM-P12XMM0-0000 to ODU G81 - 8 pin and Power (such as XT30) for connecting GMB600 to SRM Airside

{% hint style="info" %}
This guide assumes the typical system architecture where both the flight controller and the gimbal are on the same network and communicate with each other.
{% endhint %}

### 1. Explaining the architecture

<figure><img src="../../.gitbook/assets/GMB600_SRM-Airside_FC_SRM-DL_Connection-Diagram.drawio.png" alt=""><figcaption></figcaption></figure>

This architecture, where the gimbal and flight controller are on the same network, is typical for an unmanned vehicle. The SRM Airside ETH acts as a switch, bringing both of the devices and the radio on the same network. This is advantageous for two reasons:

1. The video and telemetry stream are unified on one link, eliminating any redundancies of old systems where the streams were split.
2. The gimbal has a NVidia Jetson Orin Nano integrated in it turning it into an edge-compute node. By having the flight controller on the same network as the Jetson, autonomy is enabled in the vehicle without having to strain the radio link by sending the data to the GCS for processing.



