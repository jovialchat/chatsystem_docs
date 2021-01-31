## What is Jovial Chat System?

Jovial Chat System is a **Messaging system** to bring in revolutionary scalability, reliability and Hardware/Network Failure resistance.

This project is meant to allow hosting this messaging system and allow a variety of devices to exploit it and never lose a single message.

Jovial Chat system uses combination storage and data flow mechanisms to ensure ACID-compliant delivery of messages but also ensure storage in a master-less system to prevent any bottlenecks in the Message Life Cycle. And thus allow the collection of messages by the receiver**s**.

(**yes there can be multiple receivers for the same user with proper load balancing**)

## Why Jovial Chat System?

A reliable messaging system can simply creation of a large number of reliable systems which require a reliable medium to transport of information over the internet, for connecting numerous devices and users, without any scope of failure.

The primary focus of this system is to support a variety of IoT devices and some basic interactive devices/platforms (focusing mainly on the web for now).

Presently a massive number of IoT devices are powered by MQTT, which is lightweight, fast, scalable and supports a variety of devices (ranging from IoT, web browsers, Backend Frameworks & other commonly used systems). But MQTT **lacks** reliability, Hardware & Network Failure resistance and loses functionality in the absence of Network.

Reliability, Hardware & Network Failure resistance is/will be a very important requirement of many applications, therefore while preparing them on existing Messaging platforms, one has to make custom arrangements to resolve the gaps, which is often time-consuming and difficult to design and test.

## How will Jovial Chat System cover the gaps which exist in current messaging transport systems?

