---
description: >-
  Establish an ethernet connection to a radio. Use of the wiring with or without
  evaluation board as  described in previous steps is a pre-requisite.
---

# Connecting Doodle Labs to a PC (Windows)

In order to communicate with the radio, the network adapter in the computer needs to be configured. On windows, this is done through Control Panel - Network and Internet - Network and Sharing Centre - Change Adapter Settings. There locate the network adapter associated with the Doodle Labs connection (can be easily found by unplugging and replugging the ethernet cable of the doodle labs and seeing which one disappears and reappears). Please keep in mind that it might take up to 2 minutes after giving power to the radio for it to fully boot up.

{% hint style="info" %}
You might recognize a lot of the terms used in this guide from the WiFi space, that is because the Doodle Labs radios use the same standard (with extensions) to create a network in between them.
{% endhint %}

### 1 Configuring Network Adapter

#### 1.1 Opening Network Adapter

Go into properties of the selected ethernet adapter (might require administrator permission), select the Internet Protocol Version 4 adapter and go into its properties. Change the configuration to the following:

<div align="center"><figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKjNFVuuNpc9Zwv7CH0ci%2Fuploads%2FfD9kqedmdwtBffW091Tc%2Funknown.png?alt=media&#x26;token=442751f3-41dd-453b-95c4-9a5da82c0ce1" alt="" width="563"><figcaption></figcaption></figure></div>

#### 1.2 Determining the IP Adress

Every device on the network needs to have a unique address - called an IP address. Every doodle labs radio has this address in the format 10.223.X.Y Each of the four numbers is called an octet, a group of 8 binary digits. Eight bits can represent values from 0 to 255, which is why each part of the IP address falls within that range. Every IP address consists of 4 octets divided by a dot. The subnet mask determines which part of the IP address can change, 255 meaning the octet is set and 0 meaning the octet can be any number from 0 to 255. This means that in the combination I have set it does not matter what number I write in the last two places of the IP address except the IP of the radio which would cause a conflict.

The IP address of every radio is written on its label:

{% columns %}
{% column %}
Picture 1:

<div align="left"><figure><img src="../.gitbook/assets/image (7).png" alt="" width="356"><figcaption></figcaption></figure></div>
{% endcolumn %}

{% column %}
Picture 2:

<div align="left"><figure><img src="../.gitbook/assets/image (9).png" alt="" width="333"><figcaption></figcaption></figure></div>
{% endcolumn %}
{% endcolumns %}

On newer radios, it is stated explicitly (picture 1). On older radio models, determine it from the MAC address, which is the first line written on the radio label. The last two pairs of numbers in [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) are the last two octets of the IP address (X and Y) when converted to decimal. The complete IP address is then 10.223.X.Y. it is recommended to write the IP down on the radio and put transparent tape over it to avoid it washing down.

{% hint style="success" %}
Expected Outcome: IP adress is noted down on the radio
{% endhint %}

### 2 Verifying the Connection

Firstly we will determine if connection even works by pinging it through the command prompt on

windows. Use the command ping . The correct output should look as follows:

<figure><img src="../.gitbook/assets/unknown (8).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that it can take up to 2 minutes for the radio to be available after turning on the power to it. If pinging the radio does not work, try to turn the power off, wait a 10 seconds and turn it back on. Then start trying to ping the radios almost immediately. It is possible that the radio has been configured to enter sleep mode after only a few seconds of inactivity.
{% endhint %}

{% hint style="success" %}
Expected Outcome: The IP adress of the radio can be pinged - verifies it is working.
{% endhint %}

### 3 Connecting to the Web GUI

After successfully pinging the radios, go into any web browser and type in the IP address of the radio and press enter. This will open the web Graphical User Interface (GUI), where the radio can be configured.

<div align="center"><img src="../.gitbook/assets/unknown (9).png" alt=""></div>

Some browsers will give a warning about the security of the connection. Go into advanced and proceed to the address.

<figure><img src="../.gitbook/assets/unknown (10).png" alt=""><figcaption></figcaption></figure>

There are two different combinations of usernames and passwords, depending on the firmware versions. On the older ones the username is root and the password is not set. On newer versions, the username is user with the password being DoodleSmartRadio

<figure><img src="../.gitbook/assets/unknown (11).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Expected Outcome: The web GUI can be acessed.
{% endhint %}

### 4 Debugging

{% hint style="info" icon="1" %}
If the connection to the radio is not working, you can try a full reset which will erase all of the previous settings on the radio giving you a clean configuration. The sequence to follow is different for each radio and is found in the documentation.
{% endhint %}

{% hint style="info" icon="2" %}
You can also try to ssh into the radio using the command ssh root@\<radio IP-adress>.
{% endhint %}

{% hint style="info" icon="3" %}
Lastly you can use the fallback IP address which is 192.168.153.1 to acces the web GUI. You will however need to set your computer IP address to 192.168.153.X with X being any number between 0 and 255 except 1 and a subnet mask 255.255.255.0.
{% endhint %}

{% hint style="info" %}
TIP: A good indication to what is happening inside of the radio is to monitor its current draw from the power source. If it suddenly drops below the value it started with during boot, it is likely in sleep mode.
{% endhint %}
