# Creating a Custom PX4 Build

The default PX4 software does not contain certain frames, such as the rover for ground vehicles for example. If you wish to control such a vehicle using PX4, then the best option is to build a custom version of PX4.

You can also find default pre-built versions for some of the flight controllers, they might however be missing some modules. For example the ability to control a vehicle using a joystick through ethernet. If you build your own version, you can the ones you need.

The reason they do this is that not every controller has the same amount of storage for the firmware and therefore the default size is kept to an absolute minimum.&#x20;

## Requirements:

* Linux computer
* Flight controller (this guide is using CUAV V6X)
*
