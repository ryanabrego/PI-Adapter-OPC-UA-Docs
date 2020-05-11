---
uid: OSIsoftAdapterForOPCUAClientSettingsConfiuraion
---

# OSIsoft Adapter for OPC UA client settings configuration

If you experience problems with timeouts or when OPC UA limits are exceeded in terms of browse or subscription operation, you can change the client settings configuration, which is automatically generated when a new data source is added.

## Generate default OPC UA client settings configuration file

A default OPC UA client settings file will be created if there is no OPC UA client setings configuration although a valid data source exists and the adapter is running.

Complete the following procedure for this default client settings file to be generated:

1. Add an OPC UA adapter with a unique ComponentId. For more information, see [System components configuration](xref:SystemComponentsConfiguration).
  
2. Configure a valid OPC UA data source. For more information, see [OSIsoft Adapter for OPC UA data source configuration](xref:OSIsoftAdapterForOPCUADataSourceConfiguration).

   Once you complete these steps, a default OPC UA client settings configuration file will be generated in the configuration directory for the corresponding platform.
  
   The following are example locations of the file created. In this example, it is assumed that the ComponentId of the OPC UA component is OpcUa1:

   Windows: *%programdata%\OSIsoft\Adapters\OpcUa\Configuration\OpcUa1_ClientSettings.json*
  
   Linux: */usr/share/OSIsoft/Adapters/OpcUa/Configuration/OpcUa1_ClientSettings.json*

## Configure OPC UA client settings

Complete the following procedure to configure the OPC UA client settings:

1. Using any text editor, create a file that contains an OPC UA client settings in JSON form.
    - For content structure, see [OPC UA client settings example](#opc-ua-client-settings-example).
    - For a table of all available parameters, see [OPC UA client settings](#opc-ua-client-settings-parameters).
2. Save the file, for example as _ClientSettings.json_.
3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to execute a PUT command to its appropriate endpoint:

    **Note:** The following example uses OpcUa1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    **PUT** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/ClientSettings/`

      Example using curl (run this command from the same directory where the file is located):

        ```bash
        curl -d "@ClientSettings.config.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/OpcUa1/ClientSettings"
        ```

## OPC UA client settings schema

The full schema definition for the OPC UA client settings configuration is in the _OpcUa_ClientSettings_schema.json_ here:

Windows: *%ProgramFiles%\OSIsoft\Adapters\OpcUa\Schemas*

Linux: */opt/OSIsoft/Adapters/OpcUa/Schemas*

## OPC UA client settings parameters

The following parameters are available for configuring OPC UA client settings:

**Note**: All intervals, delays, and timeouts require the string to be formatted like this: \[d:\]h:mm:ss\[.FFFFFFF\] where the items in brackets are optional. d = days, h = hours, mm = minutes, ss = seconds, F = fractional portion of a second. Example: "05:07:10:40.150" for 5 days, 7 hours, 10 minutes, 40 seconds, and .150 seconds. 

| Parameter     | Required | Type | Description |
|---------------|----------|------|-------------|
| **MaxBrowseReferencesToReturn**      | Optional | `integer` | Maximum number of references returned from browse call.  |
| **MaxNodesToBrowse**      | Optional | `integer` | Maximum number of nodes to browse in one call. |
| **ReconnectDelay**      | Optional | `TimeSpan` | Delay between reconnection attempts. |
| **RecreateSubscriptionDelay**      | Optional | `TimeSpan` | Delay between successful reconnection and subsequent subscription recreation. |
| **SessionRequestTimeout**      | Optional | `TimeSpan` | Default request timeout. |
| **ConnectionTimeout**      | Optional | `TimeSpan` | Connection timeout. |
| **SessionAllowInsecureCredentials**      | Optional | `boolean` | When set to true credentials can be communicated over unencrypted channel. |
| **SessionMaxOperationsPerRequest**      | Optional | `integer` | Default maximum operation per request. |
| **BrowseTimeout**      | Optional | `TimeSpan` | Browse operation timeout. |
| **MaxMonitoredItemsPerCall**      | Optional | `integer` | Maximum number of monitored items that can be added to subscription in one call. |
| **MaxNotificationsPerPublish**      | Optional | `integer` | Maximum notification messages in one publish message. |
| **PublishingInterval**      | Optional | `TimeSpan` | Publishing interval of the subscription. |
| **CreateMonitoredItemsTimeout**      | Optional | `TimeSpan` | Create monitored items timeout. |
| **SamplingInterval**      | Optional | `TimeSpan` | Monitored item sampling interval. |
| **MonitoredItemQueueSize**      | Optional | `integer` | Monitored item queue size. |
| **MaxInternalQueueSize**      | Optional | `integer` | Maximum number of items that can be in the adapter internal queue. |

## OPC UA client settings example

The following are examples of valid OPC UA client settings configurations:

**Minimum client settings configuration**:

```json
 {
 }
```

**Maximum client settings configuration**:

```json
{
    "maxBrowseReferencesToReturn": 0,
    "maxNodesToBrowse": 10,
    "reconnectDelay": "0:00:30",
    "recreateSubscriptionDelay": "0:00:05",
    "sessionRequestTimeout": "0:02:00",
    "connectionTimeout": "0:00:30",
    "sessionAllowInsecureCredentials": false,
    "sessionMaxOperationsPerRequest": 1000,
    "browseTimeout": "0:01:00",
    "maxMonitoredItemsPerCall": 1000,
    "maxNotificationsPerPublish": 0,
    "publishingInterval": "0:00:01",
    "createMonitoredItemsTimeout": "0:00:30",
    "samplingInterval": "0:00:00.5",
    "monitoredItemQueueSize": 2,
    "maxInternalQueueSize": 500000
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/ClientSettings  | GET | Retrieves the OPC UA client settings configuration |
| api/v1/configuration/_ComponentId_/ClientSettings  | PUT | Configures or updates the OPC UA client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | DELETE | Deletes the OPC UA client settings  configuration |
| api/v1/configuration/_ComponentId_/ClientSettings | PATCH | Allows partial updating of configured client settings fields. |

**Note:** Replace _ComponentId_ with the Id of your OPC UA component, for example OpcUa1.
