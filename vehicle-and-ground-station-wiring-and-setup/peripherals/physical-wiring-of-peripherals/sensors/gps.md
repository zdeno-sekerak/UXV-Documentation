---
description: >-
  This page describes how to physically connect sensors to a flight controller.
  For the software setup, please visit firmware guides.
---

# GPS

## GPS Connection

<figure><img src="../../../../.gitbook/assets/images.jpg" alt=""><figcaption><p>Here4 Blue GNSS</p></figcaption></figure>

{% hint style="info" %}
Connecting the GPS to the GPS\&SAFETY port enables the following

* Physical safety switch to prevent accidental arming (additional layer of safety)
* LED Pin for light status signaling (blinking = safe, solid = armed)
* Buzzer Pin for sound status signaling
{% endhint %}

It is reccomended to use the GPS\&SAFETY port on the flight controller to enable all of the following functions:

* Buzzer Pin for sound status signaling
* LED Pin for light status signaling (blinking = safe, solid = armed)
* Physical safety switch to prevent accidental arming (additional layer of safety)

{% hint style="success" %}
The same result is achieved by connection to the CAN port, but with better EMI interference. It is the most optimal way of connecting a GPS, but not all modules support it.
{% endhint %}
