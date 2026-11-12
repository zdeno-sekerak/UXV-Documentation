# Basic Autonomy Using ROS2

### Terms Explained

#### uORB Messages

Micro Object Request Broker (uORB) is an asynchronous publish-subscribe messaging middle layer. It is most commonly used in the open-source PX4 autopilot and flight control system.

It facilitates low-latency communication between different internal processes or threads in a PX4 flight controller.

#### ROS2

Robot Operating System 2 (ROS2) is a middleware framework (software layer that enables communication between different nodes): a collection of software tools, libraries and a messaging system that sits on top of a real operating system such as Linux that lets separate pieces of robot/vehicle software talk to each other.

ROS2 building blocks:

* Nodes - Each piece of a robot/unmanned vehicle process (running on camera, GPS, ESC, ...) runs independently as its own node. Nodes do not share memory or call each others functions directly. They only communicate through topics, services and actions.
* Topics - The main way nodes talk to each other is via publish/subscribe. A node publishes its data onto a topic (eg. /camera/image, /gimbal/pose) without knowing or caring who is listening. An arbitrary number of nodes can then subscribe to the streamed topic to receive the data.
* Services - For a request/response interaction (ask one question, get one answer back tasks such as "recalibrate sensor").
* Actions - For longer-running tasks, which require continuous communication until finished. They specifically provide 3 things: goal (the request), feedback (ongoing progress updates) and a result (final outcome).

In an autonomous vehicle, ROS2 would be running on the computing node, such as the NVidia Jetson.

#### DDS

Data Distribution Service (DDS) is an open standard for real-time publish-subscribe middleware that ROS2 runs on. In robots/unmanned vehicles it passes sensor data and control commands between software modules. For example between a flight controller (sends sensor data) and an autonomy node (sends command setpoints).

#### DDS-XRCE

Because a flight controller and its sensors realistically does not have the computing power DDS expects, it runs the Data Distribution Service for Extremely Resource Constrained Environments (DDS-XRCE) protocol.&#x20;

It uses an agent-client setup where the client (flight controller) sends data to the agent (jetson) which then joins the DDS network and talks to every other DDS system. The client only sends simple requests, such as which data it would like to publish and which topics it would like to subscribe to.

{% hint style="info" %}
Communication based on DDS-XRCE has no trouble running on CAN, serial, or Ethernet.&#x20;
{% endhint %}

#### MAVROS

MAVROS is the bridge that lets you keep MAVLink on the flight controller side (so GCS tools like QGroundControl still work) while giving your ROS2/DDS-based autonomy stack a way to consume that same data. This publish/subscribe layer receives MavLink commands and translates them into  ROS2 topics which use DDS the companion computer uses.&#x20;

{% hint style="info" %}
MAVROS was designed to run over Ethernet, or serial. CAN is not natively supported.
{% endhint %}
