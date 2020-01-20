## Integrating with Hero

The JSON Payload format varies for each device. The basic properties are `desiredTemperature` and `supplyTemperature`. Currently there is no standard/base format for all devices. If Hero is controlling/steering the system, we will send you a minimum of `desiredTemperature`. And you should send us the same values as confirmation that you handled our previous payload.  


Any relavent sensor data should be sent to us with an appropriate property name (we can map the value on our side), we can also send settings for devices which can be altered in the admin dashboard.   


---
## MQTT Communication for Controller Devices

### Sensor Data

Payload format in JSON

**Topic: `/<DeviceType>/<DeviceId>/Received`**

##### Message Payload:
```json
{
    "data": {
        "desiredTemperature": 80.0,
	    "supplyTemperature": 60.3
    }
}
```
---

#### (Hero -> You) Boiler Supply Temperature

**Topic: `/<DeviceType>/<DeviceId>/Publish`**

##### Message Payload:
```json
{
    "data": {
        "desiredTemperature": 80.0
    }
}
```


---
## Example Case (EasyIO)

Here is a snippet of data between our EasyIO Controllers, the specification varies for each device, dependant on its capabilities/purpose. In this case, the EasyIO controllers both control the supply temperature of the boiler, and also report the measured temperature back. For other devices these may be separate.

### **EasyIO Controller -> `/EasyIO/202/Received`**
```json
{
    "data": {
        "desiredTemperature": 39.5, 
        "supplyTemperature": 29.2, 
        "pulse": true
    }, 
    "settings": {
        "maxTemperature": 60.0, 
        "minTemperature": 10.0
    }
}
```
|Property|Type|Description|
|------------------|------|------------------------------|
|data.desiredTemperature|`float`|The current target supply temperature|
|data.supplyTemperature|`float`|The measured supply temperature|
|data.pulse|`boolean`|A "heartbeat" pulse, when true, Hero-Controller is active, when the device does not receive a new message within 16minutes, it will fall back to its own local controller, assuming the hero-controller is inactive.|
|settings.minTemperature|`float`|The minimum supply temperature|
|settings.maxTemperature|`float`|The maximum supply temperature|


---
### **Hero -> `/EasyIO/202/Publish`**


```json
{
    "data": {
        "desiredTemperature": 39.4,
        "pulse": true
    },
    "settings": {
        "maxTemperature":60.0,
        "minTemperature":10.0
    }
}
```

|Property|Type|Description|
|------------------|------|------------------------------|
|data.desiredTemperature|`float`|The desired supply temperature|
|data.pulse|`boolean`|A "heartbeat" pulse, indicating the hero-controller is active.|
|settings.minTemperature|`float`|The minimum supply temperature|
|settings.maxTemperature|`float`|The maximum supply temperature|
