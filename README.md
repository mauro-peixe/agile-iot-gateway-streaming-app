# agile-iot-gateway-streaming-app

Agile IoT Gateway app that provide API endpoints integrated with the Agile Device Manager and the Agile IDM to allow remote device management and decentralized data and computation management, following a fog computing paradigm.


## Getting Started

### Dependencies

You should have:
- A working Agile Gateway IoT installation and the required nodes installed which can be found in the following repository:
```
https://github.com/mauro-peixe/agile-node-red-nodes.git
```

- A working InfluxDB installation.

### Installation

#### Method 1:
Clone this repository into your Node-RED Library folder:

```
$ git clone https://github.com/mauro-peixe/agile-iot-gateway-streaming-app.git
```
Click Node-RED menu > Import > Library and select the folder


#### Method 2:
Copy the application configuration code to the clipboard.

Click Node-RED menu > Import > Clipboard.


### Usage

The app provides the following endpoints:

#### Configuration Endpoints

- ``[POST] /device/start``
    
    Starts the data collection in the Gateway, for the device specified.
    
    Example request:
    
    ```
    {
        "server: {{ agile_server_address }},
        "deviceId": {{ device_id }},
        "componentId": {{ component_id }},
        "name": {{ device_name }}
    }
    ```   
  
    Responses:
    ```
    Status 200: The device was started successfuly
    Status 304: The device is already on
    Status 400: There are missing configuration parameters
    ```
    
- ``[POST] /device/stop``
    
    Stops the data collection in the Gateway, for the device specified.

    Example request:

     ```
    {
        "server: {{ agile_server_address }},
        "deviceId": {{ device_id }},
        "componentId": {{ component_id }},
        "name": {{ device_name }}
    }
    ```   
     Responses:
    ```
    Status 200: The device was successfully stopped
    Status 400: There are missing configuration parameters
    Status 404: The device does not exist
    ```
      


#### Data Endpoints


- ``[GET] /device/:deviceId/component/:componentId/data``
    
    Gets the maximum value of the devices' measured data.

    Example response:
    ```
    [
        {
            "time": "2018-11-18T00:10:51.149Z",
            "componentId": "temperature",
            "deviceId": "device",
            "unit": "Celsius",
            "value": 9
        },
        {
            "time": "2018-11-18T00:10:52.149Z",
            "componentId": "temperature",
            "deviceId": "device",
            "unit": "Celsius",
            "value": 10
        }
    ]
    ```
    
- ``[GET] /device/:deviceId/component/:componentId/max``
    
    Gets the maximum value of the devices' measured data.


- ``[GET] /device/:deviceId/component/:componentId/min``
    
    Gets the minimum value of the devices' measured data.
 
     Example response:
    ```
    [
        {
            "time": "2018-11-18T00:10:51.149Z",
            "min": 2
        }
    ]
    ```
    
    
- ``[GET] /device/:deviceId/component/:componentId/average``
   
    Gets the average value of the devices' measured data.
     
    Example response:
     
    ```
    {
        "average": 2
    }
    ```



All data endpoints allow additional **filtering** to be performed through the usage of query parameters.

- ``time`` filter results from last ``time `` spent
    
    Example:
    ```
    time=1d       ## Filters entries from last day
    time=1h       ## Filters entries from last hour
    ...

    ```   


#### Authentication

All endpoints have authentication integration with the Agile Gateway IDM, so to be able to access them you should pass your auth token with every request in you ``Authorization header``.


```
Authorization: Bearer {{your_auth_token}}
``` 


## Future work
- Integration with Agile IDM authorization framework
- Develop endpoints for node configuration 
- Further development of the edge computation capabilities

## Authors

Heptasense: <a href="http://heptasense.com">www.heptasense.com</a>
