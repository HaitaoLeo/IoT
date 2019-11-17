# Sensor Configuration

In the Kognifai IoT platform there are multiple ways sensor data can be limited. There are several reasons for this, but bandwidth usage is a key concern in many IoT deployments.

In the Kognifai IoT platform the source systems will send a list of available sensors to the cloud. The data owner will use the Sensor Configuration Application in the cloud  to configure a list of replicated sensors. This is a subset of the sensors which we want to stream to the cloud.


![Sensor list exchange](.%20IoTImages/Kognifai%20IoT%20Platform%20-%20Sensor%20list%20exchange.png?raw=true)

There are several variants of this workflow at the Edge:

## Integrated Connector Sensor Configuration
- An integrated connector will query the source system for a list of available sensors which it will send to the cloud
- The data owner will use the sensor configuration application to choose the subset of available sensors he or she want to stream to the cloud
- The list of replicated sensors is sent back to the edge gateway, and the integrated connector is notified of the new list of sensors to replicate
- The integrated connector will change the list of sensors it subscribes or polls based on the newly received list from the cloud

## Connector SDK with MQTT endpoint and edge gateway filtering
- In this workflow the user has written a custom external connector which lives within the control system zone of the network. This connector uses the connector SDK to talk to the MQTT endpoint of the edge gateway.
- The connector will send a list of available sensors to the MQTT endpoint
(there is a separate MQTT topic for this)
- The edge gateway will send this list of available sensors to the cloud 
- The data owner will use the Sensor Configuration application to select a subset of the sensors to be streamed to the cloud
- The list of sensors to be replicated will be returned to the edge gateway
- The filter and dispatch component of the edge gateway will filter the incoming sensor data, dropping data from sensors that are not part of the sensor replication list

## Connector SDK with MQTT endpoint and filtering in the connector
- In this workflow the user has written a custom external connector which lives within the control system zone of the network. This connector uses the connector SDK to talk to the MQTT endpoint of the edge gateway.
- The connector will send a list of available sensors to the MQTT endpoint
(there is a separate MQTT topic for this)
- The edge gateway will send this list of available sensors to the cloud 
- The data owner will use the Sensor Configuration application to select a subset of the sensors to be streamed to the cloud
- The list of sensors to be replicated will be returned to the edge gateway
- The list of sensors to be replicated is picked up from the MQTT endpoint by the custom connector
- The connector which lives within the control system zone of the network will only send data to the MQTT endpoint which are part of the sensor replication list

## Systems which does not have a list of available sensors
- If you have to work with systems which does not provide an inventory of available sensors, it is still possible to work with the sensor configuration application in the cloud to configure which sensors to replicate
- In this workflow the data owner will define the sensors to replicate directly in the sensor configuration application. The list of sensors to be replicated will then be sent to the edge, and the workflow would otherwise work as described above.



