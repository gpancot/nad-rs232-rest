# NAD-RS232-REST

This is a small python script which does the following:

* connect via RS232 to a NAD Electronics Amplifier (tested with a C-356 model)
* connect to a MQTT broker and publish all messages from the device
* subscribe to a given topic and forward all messages as commands to the device
* implement a simple REST-API to query/configure the C-356 device

# Dependencies

This has been tested on the following setup:
* Python 3.12 
* the following installed: paho-mqtt, pyserial, flask, flask_cors, waitress
* a PL2303-type USB2Serial Adapter

# Known Limitations

The current state of this program is 'proof-of-concept', therefore it lacks many import aspects:

* proper error handling
* configuration via arguments/config file
* clean code :-)
* you can not disable mqtt
* no virtualenv

# REST API Examples

Query the current power state:
```
curl http://example.host:3333/nad/c356/v1.0/Main/Power
{
  "command": "main.power", 
  "error": 0, 
  "value": "on"
}
```

Set the current power:
```
curl -X PUT http://example.host:3333/nad/c356/v1.0/Main/Power/on
{
  "command": "main.power", 
  "error": 0, 
  "value": "on"
}
```

# MQTT Examples

- subscribe topic "NAD/C356/LivingRoom/Messages" for reply
- publish command on topic "NAD/C356/LivingRoom/Command" (query model)

```

$ mosquitto_sub -h localhost -p 1883 -v -u user -P password -t NAD/C356/LivingRoom/Messages
NAD/C356/LivingRoom/Messages Main.Model=C356BEE

$ mosquitto_pub -h localhost -p 1883 -u user -P password -t NAD/C356/LivingRoom/Commands -m "main.model?"

```

