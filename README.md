# Foundational AC Motor Concepts

## BLDC Motor

A Brushless DC (BLDC) motor consists of two main parts: permanent magnets and copper windings. The magnets sit either on the inner rotating shaft (inrunner) or on the outer rotating bell casing (outrunner), while the windings are wound onto the stationary stator.

The ESC supplies current to the windings, producing a magnetic field that continuously rotates just ahead of the permanent magnets. The magnets chase this field, and that chase produces rotation, a process called commutation.

The windings are organized into 3 phases, each switched on and off in sequence. This is why the number of slots (S) onto which the coils are wound, is always a multiple of 3. The 3-phase current must divide evenly. The number of poles (P) is always a multiple of 2, since permanent magnets always come in North-South pairs.

Motor geometry is written as XSYP, where X is the number of stator slots and Y is the number of rotor poles. A 12S14P motor, for example, has 12 winding slots and 14 magnetic poles, a ratio that directly influences torque, speed range, and smoothness.

## PMSM Motor

A PMSM motor is a 3-phase motor similar to the BLDC one. It however differs in the coil winding, magnet shape, and control algorithm complexity requirements. It is typically also more expensive and used in EV applications rather than UxV.

## Rotor and Stator

Rotor is the part of an electric motor which rotates and onto which a shaft is connected. Stator is the stationary part of the motor.

In a brushless DC motor, the pernament magnets are always mounted on the rotor, while the heavy coils are on the stator.

## Inrunner and Outrunner

An inrunner motor is one where the shaft is connected to the inner part of the motor and the entire outer casing remains stationary. On an outrunner, the setup is reversed.

An inrunner motor typically spins at very high RPM while producing little torque, unlike an outrunner, which is the opposite.

## FOC (Field-oriented Control)

FOC is a control algorithm for BLDC and PMSM motors, which defines the stator currents of a 3-phase motor as two orthogonal components which can be visualized with a vector. One component is the magnetic flux (total magnetic field passing through an area), while the other is the torque.

The electronic speed controller (ESC) follows this process (simplified):

1. Speed controller tells the system how much flux and torque it needs.
2. As the motor is spinning, it produces back-current which the ESC measures.
3. The system compares the measured current components to those referenced values and calculates the correction needed.
4. The corrections are converted back to 3-phase voltage references (a back-current decreases the voltage)
5. The controllers switches the MOSFETs on and off based on these setpoint values.

