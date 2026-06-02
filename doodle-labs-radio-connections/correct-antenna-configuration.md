---
description: >-
  This page describes the process of setting the antennas correctly for a
  desired use-case.
---

# Correct Antenna Configuration

{% hint style="info" %}
This guide assumes you already have chosen an antenna based on a previous [manual](choosing-an-antenna.md) and therefore only focuses on setting them in the correct orientation.
{% endhint %}

The Doodle Labs are built on the Mesh Rider Technology platform, which allow the use of the MIMO technology. This technique allows a data signal to be sent through multiple paths to improve reliability, being the reason why mesh rider radios have 2 antennas, each of them sending the same signal.

This guide focuses at positioning the antennas relative to each other correctly, which is entirely dependent on the antenna type.

The Ground Control Station (GCS) antenna and UAV antenna play a different game. While the first one is stationary and usually not space-restricted, the other one is.

The degree to which you will want to optimize the antenna configuration also depends on your use-case. Often a simple antenna mounted on a controller can d

### 1 Ground Control Station (GCS)

The GCS antenna is usually not limited by space and remains stationary. Therefore the usual antenna used is a directional one, if the unmanned vehicle moves in a single direction. Due to the [Fresnel Zone](foundational-antenna-concepts.md#fresnel-zone) clearance, it is also mounted on a tripod.&#x20;

#### 1.1 Diversity Logic

More advanced systems use a combination of omnidirectional and directional antennas to optimize signal reception both at takeoff where the relative position of the GCS and unmanned vehicle changes a lot and at long distance where it changes very little.

#### 1.2 Antenna Tracking

Another way advanced systems improve signal reception is through antnna tracking, where a directional antenna on the GCS changes orientation to keep pointed at the unmanned vehicle no matter their relative position.

### 2 Unmanned Vehicle

The goal for the Unmanned Vehicle antenna is to radiate signal at any orientation. Therefore a type of omnidirectional antenna is usually used in a configuration where the relative orientation of the antennas (usually a 90dg angle) to each other covers as much of the 3D space as necessary to enable signal transmission while the unmanned vehicle changes its orientation.

{% hint style="info" %}
Essentially the conclusion is to keep the antenna radiation pattern peaks pointed at each other.
{% endhint %}

{% hint style="danger" %}
Be careful about SMA and RP-SMA antenna types.
{% endhint %}
