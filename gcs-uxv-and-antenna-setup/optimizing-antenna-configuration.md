---
description: >-
  This guide focuses at positioning the antennas relative to each other
  correctly, which is entirely dependent on the antenna type, a very subjective
  matter.
---

# Optimizing Antenna Configuration

{% hint style="info" %}
This guide assumes you already have chosen an antenna based on a previous [manual](../hardware-selection/choosing-an-antenna.md) and therefore only focuses on setting them in the correct orientation.
{% endhint %}

{% hint style="warning" %}
The following configuration describes the ideal case where the Ground Control Station (GCS) antenna only needs to radiate in a single direction and is not space-constrained.
{% endhint %}

INSERT IMAGE OF SROC WITH ANTENNAS FROM SRM

The mounting requirements will depend on the mounting location

### 1 Ground Control Station (GCS)

The GCS antenna is usually not limited by space and remains stationary. Therefore the usual antenna used is a directional one, if the unmanned vehicle moves in a single direction. It is also mounted on a tripod due to the [Fresnel Zone](<../README (1).md#fresnel-zone>) clearance.

{% hint style="info" %}
This is the ideal mounting case, operational requirements may demand a less optimal mounting position, which will however result in a weaker link.
{% endhint %}

#### 1.1 Antenna Diversity

More advanced systems use a combination of omnidirectional and directional antennas to optimize signal reception both at takeoff where the relative position of the GCS and unmanned vehicle changes a lot and at long distance where it changes very little.

#### 1.2 Antenna Tracking

Another way advanced systems improve signal reception is through antenna tracking, where a directional antenna on the GCS changes orientation to keep pointed at the unmanned vehicle no matter their relative position.

### 2 Unmanned Vehicle

The goal for the Unmanned Vehicle antenna is to radiate signal at any orientation. Therefore a type of omnidirectional antenna is usually used in a configuration where the relative orientation of the antennas (usually a 90dg angle) to each other covers as much of the 3D space as necessary to enable signal transmission while the unmanned vehicle changes its orientation.

{% hint style="info" %}
Essentially the conclusion is to keep the antenna radiation pattern peaks pointed at each other. Realistically that is the best conclusion unless you want to go down the rbiithole of antennas.
{% endhint %}

{% hint style="danger" %}
Be careful about [SMA and RP-SMA](<../README (1).md#sma-and-rp-sma>) antenna types.
{% endhint %}
