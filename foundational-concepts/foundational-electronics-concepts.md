# Foundational Electronics Concepts

## Isolated Power Rail&#x20;

A power rail, which creates its own circuit essentially with the same voltage and current as the main circuit. There are no electrons passing between the rail and the main power supply. Power is transferred using another means, such as a magnetic field.&#x20;

The primary purpose of this isolation is safety and extreme noise blocking.&#x20;

## Floating Power Rail

When a rail is floating, it means that its negative terminal is simply not connected to the main system ground terminal. Its voltage is free to shift or "float" relative to the rest of the system.

It is for example used in ESCs (electronic speed controllers) to achieve a higher momentary voltage output.

The consequence is that if you are using this kind of a source, the ground wire coupled with a signal wire (in SBUS for example) muse be connected to the receiving component if it is connected to the main circuit, otherwise the communication will fail completely.&#x20;

## Failsafe

The pre-programmed behavior of an electronic system automatically executes when communication with the source of instructions is lost. For example is a signal from a GCS is lost, a UGV vehicle controller will cut throttle and put all servos in the neutral position.

