# Autonomy Using ROS2

### Terms Explained

#### uORB Messages

Micro Object Request Broker (uORB) is an asynchronous publish-subscribe messaging middle layer. It is most commonly used in the open-source PX4 autopilot and flight control system.

It facilitates low-latency communication between different internal processes or threads in a PX4 flight controller.

#### ROS2

Robot Operating System 2 (ROS2) is a middleware framework (software layer that enables communication between different nodes): a collection of software tools, libraries and a messaging system that sits on top of a real operating system such as Linux that lets separate pieces of robot/vehicle software talk to each other.

ROS2 building blocks:

* Nodes - Each independent piece of a robot/unmanned vehicle (camera, GPS, ESC, ...) runs independently as its own node. Nodes do not share memory or call each others functions directly. They only communicate through tasks.
* Topics - The main way nodes talk to each other is via publish/subscribe. A node publishes its data onto a topic (eg. /camera/image, /gimbal/pose) without knowing or caring who is listening. An arbitrary number of nodes can then subscribe to the streamed topic to receive the data.
* Services - For a request/response interaction (ask one question, get one answer back tasks such as "recalibrate sensor")
* Actions - For longer-running tasks, which require continuous communication until finished.&#x20;

