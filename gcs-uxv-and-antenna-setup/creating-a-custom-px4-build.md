# Creating a Custom PX4 Build

The default PX4 software does not contain certain frames, such as the rover for ground vehicles for example. If you wish to control such a vehicle using PX4, then the best option is to build a custom version of PX4.

{% hint style="info" %}
You can also find default pre-built versions for some of the flight controllers, they might however be missing some modules. For example the ability to control a vehicle using a joystick through ethernet. If you build your own version, you can the ones you need.
{% endhint %}

## Requirements:

* Linux computer
* Flight controller (this guide is using CUAV V6X)
*
