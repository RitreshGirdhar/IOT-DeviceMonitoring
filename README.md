# IOT-DeviceMonitoring
Sample application to capture Events from IOT devices and monitors it through ELK. 

Idea of this application is to give you enough idea to set up your own message broker and device-monitoring. 

Below are the most Popular Internet of Things Protocols and tandards Communication Technologies. 
* MQTT
* DDS
* AMQP
* Bluetooth
* Zigbee

In this tutorial i am using MQTT protocol. To capture events from devices we need mqtt broker to capture the events and store it then it would be used for further processing. 

I am using RabbitMQ as MQTT broker and to simulate behavior of IOT devices event generation i will use mosquitto publisher.

Pre-requisite
1. Set up RabbitMQ Server.
2. Install mosquitto package.
3. Set up ELK on your machine.

