# Peripherals Connection

## 1 GPS Connection

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

&#x20;Examples of other port options (for various GNSS models):

* TELEM (UART + Power and GND + RTS/CTS)
  * Connecting a GPS to this port is typically done when the primary GPS port is already occupied with (for example when blending 2 GPS positions together for redundancy)
  * There is no I2C line for compass data
* UART
  * Same result as with TELEM, but does not waste RTS/CTS lines which would otherwise be idle
* GPS
  * Same as GPS\&SAFETY, but does not enable the safety switch.

## 2 Servos and UBEC

There are two main types of servos:

* DroneCAN/UAVCAN Servos (controlled using the CAN protocol)
  * Higher EMI resistance
  * Simplified wiring, especially for longer connections, as there is one cable connecting to the flight controller
  * Higher configurability, can set torque limits, travel limits etc withouth using external programmers.
* PWM Servos (Controller using Pulse Width Modulation)
  * Cheaper and more readily available
  * Extremely simple setup

#### 2.3 VESC

#### 2.4 Flight Controller Power Module
