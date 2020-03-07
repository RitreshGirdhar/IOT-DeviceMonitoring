# IOT Device Monitoring Sample Application
Sample application to capture Events from IOT devices and monitors it through ELK or store it in events-store or some time-series database for further processing.


Pre-requisite
* Basic knowledge of Git 





Here i am taking a very basic example of capturing weather temperature city-wise. You could use it for any another event capturing.

Purpose of this application is to give you enough idea to set up your own message broker & device-monitoring.

Below are the most Popular Internet of Things Protocols and standard communication technologies :
* MQTT
* DDS
* AMQP
* Bluetooth
* Zigbee

In this tutorial i am using MQTT protocol. To capture events from IOT devices we require mqtt broker to consume the generated events and store it for further processing. 

I am using RabbitMQ as MQTT broker ([read it](https://dzone.com/articles/top-10-criteria-for-selecting-a-mqtt-broker-1))and to simulate behavior of IOT devices event generation i will use mosquitto's publisher.  
Refer [this](https://youtu.be/deG25y_r6OY) to understand basic understanding of RabbitMQ.

Pre-requisites :
1. Basic understanding of RabbitMQ and ELK.
2. Install RabbitMQ Server.
3. Install mosquitto package.
4. Set up ELK on your machine.

Rabbit MQ Configuration. 
** Make sure to enable mqtt plugin in rabbitmq using below command:
```
rabbitmq-plugins enable rabbitmq_mqtt
```

1. Creating Queue:
![Creating Queue](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-1.png)

2. Binding exchange with Queue by routing Key :
![Binding Exchange with queue](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-2.png)

3. Publising message 
![Publishing message with routing key](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-3.png)

4. Message received
![Message recieved](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Rabbitmq-4.png)


Hope you have installed mosquitto package. Now let's try to send some message through mosquitto_pub.

```
mosquitto_pub  -h 127.0.0.1 -t weather.mumbai -m "{"temperature":{"min":21,"max":29,"unit":"celsius"},"time":1568881230}" -u guest -P guest -p 1883 -d
```

Let's check weather events queue in Rabbit MQ.

![Message recieved](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Message-Received.png)

Start logstash with configuration i.e logstash-rabbitmq.conf. It is a basic configuration,which will read from RabbitMQ and dump events into weather index in Elastic Search.

paste below conf in logstash-rabbitmq.conf file
```
input {
    rabbitmq {
        host => "localhost"
        port => 15672
        heartbeat => 30
        durable => true
        exchange => "amq.topic"
        exchange_type => "topic"
        key => "weather.*"
    }
}
output {
    elasticsearch {
        hosts => "localhost:9200"
        index => "weather"
    }
    stdout {}
}
```

Start logstash 
```
logstash -f <path>/logstash-rabbitmq.conf
```

After logstash startup , you will see logstash queue get created and binded with weather* events in RabbitMQ 

![Logstash configured](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Logstash-bind.png)

Let's publish some test messages through RabbitMQ GUI

![Publish test message](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/publish-message.png)

Now let's check kibana dashboard of weather-index. As you could see in below Kibana search that our weather-mumbai event received by Elastic-Search through logstash.

![Kibana received Message](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/Kibana-read-weather-index.png)

Now let's test it through MQTT client/IOT Device (in our case its mosquitto_pub)

```
$ mosquitto_pub  -h 127.0.0.1 -t weather.mumbai -m '{"temperature":{"min":21,"max":32,"unit":"celsius"},"timestamp":"2019-09-19T18:59:00"}' -u guest -P guest -p 1883 -d
```

![Kibana received Message 1](https://github.com/RitreshGirdhar/IOT-DeviceMonitoring/blob/master/images/mosquito-msg-consumer.png)



