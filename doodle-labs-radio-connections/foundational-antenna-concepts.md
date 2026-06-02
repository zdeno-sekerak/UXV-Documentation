---
description: >-
  Antennas are a really complex topic, this guide aims to develop a basic
  understanding for the purpose of setting up a communication link between
  (Doodle Labs) inradios.
---

# Foundational Antenna Concepts

### Radiation Pattern

The RF signal is essentially a high frequency sound. And in a similar way normal sound can be focused when coming out of a speaker, RF signal can also be focused using an antenna. When sound is focused in a certain direction, it can be louder in that direction, however it will be quiter from all other ones.

The radiation pattern is the general term for a 3D graphical representation of how such a power distribution might look. as shown by some examples.

<figure><img src="../.gitbook/assets/6b67a936-d887-4feb-b131-96dc953cab32.jpg" alt=""><figcaption></figcaption></figure>

Antenna radiation patterns can be quite nuanced so if you are trying to set up an antenna to get the best connection, it should be advantageous to look up the radiation pattern for it specifically.

### Antenna Gain

Antenna gain is a single number explaining the strongest direction of the radiation pattern, compared to an ideal antenna radiating equally in all directions. The function expressing the antenna gain is logarithmic.

The choice of appropriate antenna gain is dependent on the use-case.

<figure><img src="../.gitbook/assets/antenna-gain-explanation (1).jpg" alt=""><figcaption><p>An image representing the cross-section of an antenna radiation pattern for a directional antenna (this image would be different for an omnidirectional antenna for example - it would be a squished doughnut)</p></figcaption></figure>

It is pretty common to combine two types of antennas in one link, for example to use a high-gain for the GCS antenna as the drone will generally only fly in one direction, and a low-gain antenna on the UAV so that the signal signal is independent of the drone orientation. Some setups even combine two types of antennas on the receiver side.

### Antenna Polarization - Linear or Circular

{% hint style="info" %}
The antenna polarization pattern is independent of the antenna radiation pattern.
{% endhint %}

The radio signal consists of an electric and magnetic field oscillating perpendicular to each other. When describing antenna polarization, we are focusing on the direction of the electrical field.

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/Circular_Polarization_Linear_Polarized_Light_Entering_Quarter_Wave_Plate_Components.svg.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/Circular.Polarization.Circularly.Polarized.Light_Without.Components_Right.Handed.svg.png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

In the image on the left, we can see a linearly polarized antenna, while the circular antenna is on the right side. The main thing to look at here, without going too much into antenna theory, is the red plane. In the case of the linearly polarized antenna, it is the 45 degree angle one, and in the case of circularly polarized one, it is the cylinder. That is the plane depicting the signal.

Linear polarization can, in general terms, provide extra range, as the energy is focused on a single plane, not a cylindrical pattern. On the other hand, it also means that both of the transmitting and recieving antennas need to be alligned at the correct angle in order to achieve maximum overlap. In practical terms, this can mean that when a drone is turning in the sky, it may reduce link quality significantly.

For this reason, cyllindrically polarized antennas tend to be more popular for drone pilots, as they will get the same signal strength, no matter the angle of a drone.

It is also important to note, that there are two types of circular polarization antennas, left hand (LHCP) and right hand (RHCP) ones. This can be imagined as the ''spin'' direction of the circular signal and it is important that both of the antennas are using the same one.

### Single-Band X Multiband

Radios operate at a certain frequency, for which a dedicated antenna has to be selected. A single-band antenna should be used when operating at a single frequency. However, if the radio is intended to operate across multiple frequencies, for example, when frequency hopping in environments with RF jamming, a multiband antenna is required. The tradeoff is that a multiband antenna is not optimized for any single frequency and will therefore perform worse than a dedicated single-band antenna at each respective frequency.

### Fresnel Zone

Intuitively, it would make sense that radio waves only need the line-of-sight without obstruction to achieve the best result. However due to the complexity of radio links, the zone, which must stay without obstruction takes a 3D ellipsoid shape between the transmitter and receiver.

The illustration below shows different ways a radio signal can be obstructed.

<figure><img src="../.gitbook/assets/Screenshot 2026-05-26 152644.png" alt="" width="499"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-05-26 152633.png" alt="" width="488"><figcaption></figcaption></figure>

### Power Level (unit: dBm)

dBm is the standard unit used to express the transmit power of a radio (among other things). Because the scale spans an enormous range from microwatts (about the level of a weak received signal) to wats (transmit power), the unit is logarithmic, meaning every +3dBm doubles the power.

### Antenna Frequency

The frequency is the optimal wave frequency the antenna is designed to receive.

Generally a lower frequency means that the waves themselves are longer and travel further, in both open and congested areas as object penetration is also exceptional.

On the other hand, a higher frequency bands have vastly more spectrum available in absolute terms, so there's more room to allocate wide channels. This results in a higher data throughput.

An antenna is optimized for either one or several frequency bands. The tradeoff for having an antenna optimized for several of them is of course that the performance for any single frequency is going to be lower.

### Frequency Congestion

Some frequencies may have more data already travelling through them from other sources, which will introduce more noise into your communication. The Doodle Labs radios offer a spectrum scan function, which will allow you to select the least congested channel.

### Bandwidth

Antenna bandwidth defines the range of frequencies over which the antenna can effectively transmit signal.

### SMA & RP-SMA

SMA and RP-SMA are antenna connector types. They look very similar at first glance, but they have the pin and hole swapped and will therefore not work unless used with a SMA-to-RP-SMA adapter. Users should be careful to get the distinction right.

{% columns %}
{% column valign="middle" %}
<figure><img src="../.gitbook/assets/RP-SMA-Femal.jpg" alt=""><figcaption><p>RP-SMA Female Connector</p></figcaption></figure>
{% endcolumn %}

{% column valign="middle" %}
<figure><img src="../.gitbook/assets/SMA-Female.jpg" alt=""><figcaption><p>SMA Female Connector</p></figcaption></figure>
{% endcolumn %}
{% endcolumns %}
