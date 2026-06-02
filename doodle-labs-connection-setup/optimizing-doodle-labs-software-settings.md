---
description: >-
  This page explains how to optimize a link between Doodle Labs radios to
  achieve a high quality connection.
---

# Optimizing Doodle Labs Software Settings

In the previous step, we set up the RF link in a way that it works, most users will however wish to optimize the link.

{% hint style="info" %}
Based on a previous guide, both of the radios web GUI are accessible while only one of the radios is connected to the computer physically. The radio link enables information transfer from the other radio.
{% endhint %}

{% hint style="info" %}
In order to optimize the link, it is recomended to mount the antennas in the final configuration, eg. on the UAV/UGV and the controller.
{% endhint %}

### 1 Check Antenna Quality

Start by navigating to Advanced Settings - Network configuration - Wireless in the web GUI on both of the radios. Then select edit in the link between the radios. A menu will appear.&#x20;

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Make sure that the difference in the signal of antennas of one radios is less than 3 dBm (Every 3-dB coaxial cable loss results in 3-dB loss in the TX power and 3-dB loss in the RX signal which results in 2x reduction in the operating range). As the scale is logarithmic even seemingly small differences can be significant. UXV for example found an antenna was damaged because of the mismatch.

In the image you can see an example of a healthy link:

<figure><img src="../.gitbook/assets/unknown (5).png" alt=""><figcaption></figcaption></figure>

Continue by setting the Transmit Power if needed, the default automatic setting is however enough for most applications.

If you wish to analyze the link further, you can use the Doodle Labs Technical Library [range estimation tool](https://learn.doodlelabs.com/range-estimation-tool) and [throughput estimation tool](https://learn.doodlelabs.com/throughput-estimation-tool).

{% hint style="info" %}
To further investigate radio link, it is also useful to run an analysis in the application you are using. For example for RC aircraft control a 25% packet loss is high as the software quite often waits for confirmation from the airplane that the packet was received because it cannot operate on uncertain data. However for video streaming a 50% loss might be acceptable as it does not wait for any confirmation and it is advantageous to have a higher data throughput.
{% endhint %}
