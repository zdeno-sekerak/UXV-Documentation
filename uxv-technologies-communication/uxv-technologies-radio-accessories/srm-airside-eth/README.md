---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
---

# SRM Airside ETH

The SRM Airside ETH is a [Linux](https://en.wikipedia.org/wiki/Linux)-based computer running [OpenWrt](https://en.wikipedia.org/wiki/OpenWrt). It is installed on a drone or other vehicle to provide compatibility with [SRM radios](../../uxv-technologies-srm-radios/).

<figure><img src="../../../.gitbook/assets/image__5_-removebg-preview.png" alt=""><figcaption></figcaption></figure>

The SRM Airside ETH can be configured as a [network switch](https://en.wikipedia.org/wiki/Network_switch) to connect an [SRM radio](../../uxv-technologies-srm-radios/swappable-radio-modules-srm-radios/) with other [Ethernet](https://en.wikipedia.org/wiki/Ethernet) devices, such as a [camera gimbal](/broken/pages/M0yaFCR0HYV4lMNHHpNt) or an ethernet-enabled flight controller.

## Hardware Overview

The SRM Airside ETH provides the following physical interfaces:

* **2 × Ethernet ports** via ODU G81 8-pin connectors.
* **1 × Ethernet port and power output** to the SRM radio via the [SRM interface](../../uxv-technologies-srm-radios/swappable-radio-modules-srm-radios/srm-interface.md).
* **1 × Power input and UART** via an ODU G80 10-pin connector.

{% columns %}
{% column width="58.333333333333336%" %}
<figure><img src="../../../.gitbook/assets/uxv_technologies_airside_eth (3).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column width="41.666666666666664%" %}
<figure><img src="../../../.gitbook/assets/srm-airside-eth (2).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

By default, the first Ethernet port on the side of the unit is configured as a **configuration port**, providing direct access to the SRM Airside ETH. The second Ethernet port is configured as a **passthrough port**, providing access to the radio installed in the SRM slot.

{% columns %}
{% column %}
<figure><img src="../../../.gitbook/assets/uxv_technologies_airside_eth_with_descriptions.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}

{% endcolumn %}
{% endcolumns %}

## Connection to SRM Airside ETH

{% hint style="info" %}
The following instructions assume that the SRM Airside ETH is using its default network configuration. Devices with modified network settings may require a different configuration.
{% endhint %}

1. Connect the **Configuration** port (ODU S11YAR 8-pin connector, shown above) to an Ethernet port on the computer.

<figure><img src="../../../.gitbook/assets/img_1710-removebg-preview (2).png" alt=""><figcaption></figcaption></figure>

2. Connect a **5–20 V** power supply to the power input (ODU S10YAR 10-pin connector) on the SRM Airside ETH.

<figure><img src="../../../.gitbook/assets/uxv_technologies_srm_airside_and_radio_b_and_w.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To verify that the SRM Airside ETH is powered, insert an SRM radio into the radio slot and set the power switch to **ON**. Check that the radio's power status LED is illuminated.
{% endhint %}

3. Configure the computer's network settings. Ensure that the IP address of the computer is on the same subnet as the SRM Airside ETH.
4. Enter the IP Address of the SRM Airside ETH in a web browser to open the web configuration interface.

<figure><img src="../../../.gitbook/assets/Screenshot 2026-08-26 093112.png" alt=""><figcaption></figcaption></figure>

Alternatively, connect to the SRM Airside ETH using [SSH](https://en.wikipedia.org/wiki/SSH) with the following command:

```bash
ssh root@192.168.1.1
```

{% hint style="warning" %}
If the SRM Airside ETH is accessible via SSH but not through the web interface, try clearing the browser cache. Some browsers may block access after repeated failed connection attempts.
{% endhint %}

## Connection to SRM Radio

By default, the second Ethernet port is configured as a passthrough connection to the SRM radio.

Connect the computer to this port using the same ODU S11YAR 8-pin to RJ45 Ethernet cable.

For instructions on configuring the radio, see [Connecting to a Doodle Labs Radio](../../standalone-radio-guides/connecting-to-a-doodle-labs-radio.md).

## Integration Notes

When integrating the SRM Airside ETH into a drone or other vehicle, it is recommended to configure the device as an [Ethernet switch](srm-airside-eth-as-a-switch.md) to enable the use of both Ethernet ports.
