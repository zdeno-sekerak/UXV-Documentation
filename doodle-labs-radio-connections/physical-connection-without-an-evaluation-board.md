# Physical Connection without an Evaluation Board

Connecting the radio without an evaluation board is useful when:

* You wish to configure it and are missing an evaluation board
* You are integrating the configured radio into a product

Parts:

* 2 Doodle Labs Radios
* Connectors for the Radios
* Antennas for the Radios
* Ethernet Cable

Tools:

* Power Source - Battery or Bench Power Supply
* Multimeter
* Soldering Iron

The main challenge of connecting a radio without an evaluation board is finding the correct pins and connecting them together. It is therefore key to locate the documentation for the radio you have, the process of which was described in this [guide](physical-connection-with-an-evaluation-board.md).

To power the radios, locate the PWR and GND pins. Most of the newer radios have more than one pin for PWR and GND. This is to distribute the load across several cables and pins as the connectors have a safe current rating of 1A per contact. Plug in a cable into each of the connectors.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Then use tweezers to remove te cables from the free connector. Insert the shart end under a plastic hinge above the metal and lift it up. Then it is possible to pull the cable out.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

You should end up with something like this.

<figure><img src="../.gitbook/assets/IMG_0474.jpg" alt=""><figcaption></figcaption></figure>

Now connect all of the PWR pins to the + pin and GND to the - pin on the power supply/battery. The recommended way to do this is to solder all of the PWR or GND wires onto two thicer cables going into the power supply.&#x20;

For a legacy radio model, which only requires one PWR and GND cable respectively, a connection might look something like the following.

<figure><img src="../.gitbook/assets/IMG_0476 (1).jpg" alt=""><figcaption></figcaption></figure>

The next step is to connect ethernet pins to a cable. In this case, UXV Technologies will be using a CUAV V6X flight controller with networking capablities and will wish to connect a radio using a 4-Pin JST-GH connector. Another common method of connecting ethernet is using the RJ-45 connector, which is easier to connect to a computer as a lot of them have a physical RJ-45 port (also simply called Ethernet Port).

Start by locating the ETH pins in the documentation and on the radio.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Then find the pinout of the target connector, in our case it is the ETH port on the CUAV V6X.

{% columns %}
{% column %}
<figure><img src="../.gitbook/assets/image (37).png" alt="" width="189"><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../.gitbook/assets/46e66c1fcbe21f8910b95691846599ba.jpg" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

