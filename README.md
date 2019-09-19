# IOT-DeviceMonitoring
Sample application to capture Events from IOT devices and monitors it through ELK or store it in events-store or some time-series database for further processing.

Here i am taking very basic example of weather temperature capturing city wise. You could use it for any another event capturing.

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

Rabbit MQ :
1. Creating Queue:
![Creating Queue](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-1.png)

2. Binding exchange with Queue by routing Key :
![Binding Exchange with queue](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-2.png)

3. Publising message 
![Publishing message with routing key](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-3.png)

4. Message received
![Message recieved](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-4.png)


Hope you have install mosquitto package Let's try to send some message through mosquitto_pub.

```
mosquitto_pub  -h 127.0.0.1 -t weather.mumbai -m "{"temperature":{"min":21,"max":29,"unit":"celsius"},"time":1568881230}" -u guest -P guest -p 1883 -d
```

Lets check weather events queue in Rabbit MQ 

![Message recieved](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Message-Received.png)

