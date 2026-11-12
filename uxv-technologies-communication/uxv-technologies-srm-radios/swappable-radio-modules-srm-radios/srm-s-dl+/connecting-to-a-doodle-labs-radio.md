# Connecting to a Doodle Labs radio

To connect to a Doodle Labs radio through ethernet using IPv4 from a computer, follow the steps described below.

The radio runs [Mesh Rider OS](https://doodlelabs.com/technology), Doodle Labs' [OpenWrt](https://en.wikipedia.org/wiki/OpenWrt)-based firmware.

{% hint style="info" %}
This guide assumes a physical connection is established.
{% endhint %}

## Radio IP Adress

The default [IPv4 adress](https://en.wikipedia.org/wiki/IP_address) of a Doodle Labs radio is `10.223.1.1/16` on the `225.225.0.0` [subnet](https://en.wikipedia.org/wiki/Subnet#Internet_Protocol_version_4).

{% hint style="info" %}
Onder Doodle Labs radios also have a fallback IP `192.168.153.1` , which can be used to connect and change settings on the radio.
{% endhint %}

In order to connect to a Doodle Labs radio, you have to find its exact IP address. Some of the ways to find the adress if the radio is in default settings are listed below:

* Device label (newer models): Read the IP address printed directly on the standalone radio label.
* MAC address calculation: Derive the `10.223.x.y` address from the radio's MAC address, where `x` and `y` are the last two hexadecimal octets converted to decimal.
* Fallback IP (older models): Connect using the fallback IP `192.168.153.1` and view the assigned address in the settings.

If the radio is in non-default settings, try using the `nmap` scan on a Linux device.

Otherwise contact UXV Technologies for SRM-S-DL+ and Doodle Labs for standalone radio, for support

## Computer Setup

{% hint style="warning" %}
No single configuration is correct. Some users may require different settings for various use cases. This guide only the setup of a radio with the default IP adress `10.223.1.1/16`.
{% endhint %}

### Windows

{% hint style="info" %}
This setup has been verified to work with Windows 11. Older versions might now work as described.
{% endhint %}

1. Navigate to Windows Network Adapter setting, select Internet Protocol version 4 (IPv4) adapter and open properties.
2. Select "Use the following IP address:".
3. Set the IP adress and the subnet mask to your required settings based on your radio configuration.

Below is an example of the settings for a radio in the default configuration:

<div align="center"><figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKjNFVuuNpc9Zwv7CH0ci%2Fuploads%2FfD9kqedmdwtBffW091Tc%2Funknown.png?alt=media&#x26;token=442751f3-41dd-453b-95c4-9a5da82c0ce1" alt="" width="563"><figcaption></figcaption></figure></div>

### Linux

Set the IP adress of the computer to an IP adress on the same subnet (`255.255.0.0` is the default) as the radio. For a Doodle Labs radio in the default configuration use `10.223.1.1/16`.

```
sudo ip addr add <IP_ADDRESS>/<CIDR> dev <INTERFACE>
```

### Verifying the Connection



