# Common Fields

## Required

* [MessageType](#messagetype) ```string```
* [Spec](#spec) ```string```
* [DeviceId](#deviceid) ```string```
* [MessageId](#messageid) ```string```

### MessageType
```string``` 

Contains a string of the message type, as per the details in the individual message types specification. e.g. "SpotTelemetryMessage", "EventMessage"
### Spec
```string``` = "1.1.0.0"

Spec is important for versioning of message processing where breaking changes are made to the standard.
### DeviceId
```string``` 

Uniquely identifies each device on IoT service. On Azure IoT Hub, this needs to match with the [DeviceId]. 
### MessageId
```Int32```

MessageId is a sequential counter of messages on the device. It can be duplicated (restart at 0, reset on firmware change), but is used for message ordering and fault identification, so ensuring that it is used correctly improves overall quality.

MessageId is required for messages originating on the device (Origin=2), but not where messages for a device are generated as a side-effect of processing (server-side, edge gateways, etc.) due to other processes not being able to determine what the next MessageId would be.

## Optional

* [Tags](#tags) ```string[]```
* [Origin](#origin) ```Int32```
* [Adjustments](#adjustments) ```object```

### Tags
```string[]```

Tags are set on messages to aid cold analytics. For example, messages can be tagged as _Test_ to ensure that setup and test message to do not affect real-world analytics.

### Origin
```Int32```

Used for marking of ‘derived’ messages, where the origin of the message is not the active device. Important for understanding (e.g. count statistics) device versus server-side event detection.

### Adjustments
```object[]```

In cases where devices are sending up incorrect data, due to a fault or firmware bug, it is preferred to adjust the data in order to prevent erroneous measurements or to fail the entire message. This is especially valid when other measurements or parts of the message are valid, and when the rollout of updates to devices takes a long time, so continuous manual resubmission of failed messages cannot be done. The adjustment structure preserves the original data and allows for monitoring and logging of automatic adjustments made.

Adjustments can be made anywhere, including the device, or from a UI, but will mostly be done in a field or cloud gateway or on the gatekeeper.

#### Format
Array of Adjustment object

* AdjustmentDateTime ```string```
* Processor ```string```
* ReasonCode ```string```
* Reason ```string```
* AdjustmentPropertyPath ```string```
* OriginalFragment ```object```

#### Example
In the example below, a measurement value of null is sent, which is not possible, so removed from the measurements.
```JSON
"Adjustments": [
    {
      
      "AdjustmentDateTime": "2016-11-03T11:20:43Z",
      "Processor": "SK-Gateway",
      "ReasonCode": "SK-Zero-Amps",
      "Reason": "Heater null amps removed",
      "AdjustmentPropertyPath": "ComponentMeasurements[2].Measurements[0]",
      "OriginalFragment": 
        {
          "Key": "Phase1",
          "UnitOfMeasure": "Amps",
          "Value": "null"
        }
    }
  ]
```