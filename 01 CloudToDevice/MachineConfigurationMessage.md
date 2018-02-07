# Machine Configuration Message
## Usage
The list of components attached to a machine, or a model of a machine, is usually administered in the cloud. However, in cases where the components of the machine can change in the field, such as when an additional component is plugged in and detected by a device, the device needs to be able to communicate the change in the components connected to the machine.

## Format
* [MessageType](#messagetype) ```string```
* [Spec](#spec) ```string```
* [DeviceId](#deviceid) ```string```
* [MessageId](#messageid) ```Int32```
* [DateTime](#datetime) ```string```
* [MachineModel](#machinemodel) ```string```
* [MachineSerialNumber](#machineserialnumber) ```string```
* [MachineOperation](#machineoperation) ```byte```
* [Components](#components) ```object[]```
    * [ComponentCode](#componentscomponentcode) ```string```
    * [PartNumber](#componentspartnumber) ```string```
    * [ComponentName](#componentscomponentname) ```string```
    * [SerialNumber](#componentsserialnumber) ```string```
    * [UnitsOfMeasure](#componentsunitsofmeasure) ```string```
    * [Properties](#componentsproperties) ```object[]```
        * [Key](#componentspropertieskey) ```string```
        * [Value](#componentspropertiesvalue) ```string```
    * [Components](#components) ```object[]```



### MessageType
```string``` = "BasicDiagnosticMessage"
### Spec
```string``` = “1.1.0.0”
### DeviceId
```string``` 
### MessageId
```Int32```
### DateTime
```string``` in DateTime Format ```yyyy-MM-ddTHH:mm:ssZ```
### MachineModel
```string```

Machine information is optional. Adding machine data will force a check and insert/update server-side. Insert/update depends on MachineOperation
### MachineSerialNumber
```string```

Machine information is optional. Adding machine data will force a check and insert/update server-side. Insert/update depends on MachineOperation
### MachineOperation
```byte```

0 Update – updates the serial number and model of the machine associated with this device. 

1 Add – Adds a new machine and associates this device with that machine.
### Components
```object[]```

The component list is a hierarchical structure 
### Components/ComponentCode
```string```
### Components/PartNumber
```string```
### Components/ComponentName
```string```
### Components/SerialNumber
```string```
### Components/UnitsOfMeasure
```string```
### Components/Properties
```object[]```
### Components/Properties/Key
```string```
### Components/Properties/Value
```string```

## Sample
```JSON
{
  "MessageType": "MachineConfigurationMessage",
  "Spec": "1.1.0.0",
  "DeviceId": "AF0002",
  "MessageId": 101,
  "DateTime": "2016-03-12T12:40:42Z",
  "Components": [
    {
      "ComponentCode": "Pump001",
      "PartNumber": "AV-RSU-998-Z",
      "ComponentName": "Pump 1",
      "UnitsOfMeasure": "PSI"
    },
    {
      "ComponentCode": "RefrigeratorBase",
      "ComponentName": "Refrigerator Base",
      "Components": [
          {
            "ComponentCode": "RB-Pump002",
            "ComponentName": "Refrigerator Base Pump 2",
	    "SerialNumber": "AF-987EG-0034",
            "UnitsOfMeasure": "PSI"
          },
          {
            "ComponentCode": "RB-Valve01",
            "ComponentName": "Refrigerator Base Valve 1",
            "Properties": [
              {
                "Key": "Channel",
                "Value": "65"
              },
              {
                "Key": "PowerOnState",
                "Value": "Open"
              }
            ]
          }
      ]
    }
    ]
}
```

## Server-side validations
1.	[DateTime](#datetime): Required. Standard DateTime validation.
2.	If [Components](#components) not null
    1. [ComponentCode](#componentscomponentcode): Required.
    2. [ComponentName](#componentscomponentname): Required.
3. If [MachineOperation](#machineoperation) not null
    1. [MachineModel](#machinemodel) or [MachineSerialNumber](#machineserialnumber) required
4. If [MachineModel](#machinemodel) or [MachineSerialNumber](#machineserialnumber) not null
    1. [MachineOperation](#machineoperation) Required
5. If [Components/Properties](#componentsproperties) not null
    1. [Components/Properties/Key](#componentspropertieskey) Required
    2. [Components/Properties/Value](#componentspropertiesvalue) Required