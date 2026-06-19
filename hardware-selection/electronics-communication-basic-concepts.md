# Serial Communication Basic Concepts

## Synchronous X Asynchronous

A digital communication is just a combination of `HIGH` and `LOW` pulses sent across a wire. If there is a constant high pulse, the receiving side does not know if it is just one or several HIGH pulses in a row. There are two common ways communication protocols have been dealing with this issue.

Synchronous communication protocols share an additional clock wire. One of the devices is defined as master and sends high and low pulses at a constant rhythm. This makes it very easy for the other device to interpret the data and the communication speed is exceptionally high.

Asynchronous communication relies on the internal clocks of the two devices, which are not always in sync. Therefore it sends framing (start and stop) bits, which define the start and end of a data frame. The advantage of this protocol is that it requires only one wire (for one-way communication) and a common ground.

## UART

Hardware communication protocol used to send serial data between two devices. It uses 3 wires - Transmit (TX), Receive (RX) and Ground (GND). The wires are connected cross-wise, meaning TX pin of Device A is connected to RX pin of Device B.

The signal has no shared clock and instead both of the devices must agree on a baud rate at which the bits are transferred, for example a baud rate of 115 200 means 115 200 bits per second. The most common baud rates are $$2^{n} * 16$$.

In standard UART, the IDLE state is HIGH (1). If the IDLE state was LOW, a broken line would not be distinguishable from an IDLE line, which can be relevant in certain situations.&#x20;

## Parity

Single bit appended to the end of every UART frame which gives the reciever a basic way of detecting an error. Before transmitting a bite (8 bits) of data, the sender counts the number of 1s and then appends the parity bites which makes the count either even or odd.&#x20;

The following table gives an example of how a parity is commonly defined in UART. The first number determines the number of data bits (carrying information), the middle letter defines the parity and the last number defines the number of stop bits.

| Setting | Meaning                                             |
| ------- | --------------------------------------------------- |
| 8N1     | No parity at all.                                   |
| 8E1     | Even parity, sets the total number of 1 to be even. |
| 8O1     | Odd parity, sets the total number of 1 to be odd.   |

## Start and Stop Bits

For the start bit, the transmitter sets the first bit to the opposite state of IDLE, which signals to the receiver that a byte is coming. The start bit is always one bit long.

The duration of the stop bits can differ for various UART configuration, however the concept is the same. The transmitter pulls the line back to the idle state for the duration of the stop bit, signaling an end of a frame.

## SBUS

SBUS is a protocol used to send data over UART with a baud rate of 100 000 bps developed by Futaba for RC airplanes. It uses inverted logic, meaning the IDLE line is LOW and the configuration is specified at 8E2.
