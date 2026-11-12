# Connecting to a Doodle Labs Radio

This guide describes how to connect a computer to a Doodle Labs radio over Ethernet using IPv4.

Doodle Labs radios run [Mesh Rider OS](https://doodlelabs.com/technology), an [OpenWrt](https://en.wikipedia.org/wiki/OpenWrt)-based firmware developed by Doodle Labs.

{% hint style="info" %}
This guide assumes that a physical connection between the computer and the radio has already been established.
{% endhint %}

## Radio IP Address

The default [IPv4 address](https://en.wikipedia.org/wiki/IP_address) of a Doodle Labs radio is `10.223.1.1/16`, with a subnet mask of `225.225.0.0`.

{% hint style="info" %}
Older Doodle Labs radios also have a fallback IP address of `192.168.153.1`, which can be used to access the radio and modify its configuration.
{% endhint %}

To connect to the radio, you must first determine its IP address.

For radios using the default configuration, the IP address can be determined using one of the following methods:

* **Device label (newer models):** Read the IP address printed on the standalone radio's label.
* **MAC address calculation:** Derive the `10.223.x.y` address from the radio's MAC address, where `x` and `y` correspond to the last two hexadecimal octets converted to decimal.
* **Fallback IP (older models):** Connect to the radio using `192.168.153.1` and check its assigned IP address in the radio settings.

If the radio is not using the default configuration, try using `nmap` on a Linux computer to scan for its IP address.

If you cannot determine the IP address, contact UXV Technologies for support with the SRM-S-DL+ or Doodle Labs for support with standalone radios.

## Computer Setup

{% hint style="warning" %}
The required network configuration depends on the application.
This guide covers only the configuration of a radio using the default IP address `10.223.1.1/16`.
{% endhint %}

### Windows

{% hint style="info" %}
This procedure has been verified on Windows 11.
Older Windows versions may require different steps.
{% endhint %}

1. Open the Windows network adapter settings and select **Internet Protocol Version 4 (TCP/IPv4)** in the adapter properties.
2. Select **Use the following IP address**.
3. Configure the IP address and subnet mask according to the radio's network configuration.

The following example shows the settings for a radio using the default configuration:

<div align="center"><figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKjNFVuuNpc9Zwv7CH0ci%2Fuploads%2FfD9kqedmdwtBffW091Tc%2Funknown.png?alt=media&#x26;token=442751f3-41dd-453b-95c4-9a5da82c0ce1" alt="" width="563"><figcaption></figcaption></figure></div>

### Linux

Configure the computer's IP address to be on the same subnet as the radio.
The default subnet mask is `255.255.0.0`.

For a radio using the default IP address `10.223.1.1/16`, assign the computer a different IP address within the same subnet.

Use the following command to assign an IP address to the network interface:

```
sudo ip addr add <IP_ADDRESS>/<CIDR> dev <INTERFACE>
```

### Verifying the Connection
