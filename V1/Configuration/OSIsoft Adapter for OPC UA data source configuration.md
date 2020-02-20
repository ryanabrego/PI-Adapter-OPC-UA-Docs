---
uid: OSIsoftAdapterForOPCUADataSourceConfiguration
---

# OSIsoft Adapter for OPC UA data source configuration

To use the OPC UA adapter, you must configure the data source from which it will be polling data.

## Configure OPC UA data source

**Note:** You cannot modify OPC UA data source configurations manually. You must use the REST endpoints to add or edit the configuration.

Complete the following procedure to configure the OPC UA data source:

1. Using any text editor, create a file that contains an OPC UA data source in JSON form.
    - For content structure, see [OPC UA data source example](#opc-ua-data-source-example).
    - For a table of all available parameters, see [OPC UA data source parameters](#opc-ua-data-source-parameters).
2. Save the file, for example as _DataSource.config.json_.
3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to execute a POST command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<adapterId>/DataSource/`. 

      **Note:** The following example uses OpcUa1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration).
    
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using curl (run this command from the same directory where the file is located):

    ```bash
    curl -d "@DataSource.config.json" -H "Content-Type: application/json" -X POST "http://localhost:5590/api/v1/configuration/OpcUa1/DataSource"
    ```

**Note:** After you have completed data source configuration, the next step is to configure data selection. You can either have a default data selection file generated or you can create the data selection file yourself. For more information, see [OSIsoft Adapter for OPC UA data selection configuration](xref:OSIsoftAdapterForOPCUADataSelectionConfiguration).

## OPC UA data source schema

The full schema definition for the OPC UA data source configuration is in the _OpcUa_DataSource_schema.json_ here:

Windows: *%ProgramFiles%\OSIsoft\Adapters\OpcUa\Schemas*

Linux: */opt/OSIsoft/Adapters/OpcUa/Schemas*

## OPC UA data source parameters

The following parameters are available for configuring an OPC UA data source:

| Parameter | Required | Type | Nullable | Description |
|-----------|----------|------|----------|-------------|
| **EndpointURL** | Required | `string` | No | The endpoint URL of the OPC UA server in opc.tcp format. The following is an example of the URL format: opc.tcp://OPCServerHost:Port/OpcUa/SimulationServer<br><br>**Note:** If you change the EndpointURL on a configured OPC UA adapter that has ComponentID_DataSelection.json file exported, you need to remove the _ComponentID_DataSelection.json_ file from the configuration directory to trigger a new browse (export).|
| **UseSecureConnection**|Optional | `boolean` | No | When set to true, the OPC UA adapter connects to a secure endpoint using OPC UA certificate exchange operation. The default is true. When set to false, the OPC UA adapter connects to an unsecured endpoint of the server and certificate exchange operation is not required.<br><br>**Note:** OSIsoft recommends setting this option to false for testing purposes only.|
| **UserName** | Optional | `string` | Yes | User name for accessing the OPC UA server. |
| **Password** | Optional | `string` | Yes | Password for accessing the OPC UA server.<br><br>**Note:** OSIsoft recommends using REST to configure the data source when the password must be specified because REST will encrypt the password. If you do not use REST, the plain text password will be stored on-disk.|
| **RootNodeIds** | Optional | `string` | Yes |List of comma-separated NodeIds of those objects from which the OPC UA adapter browses the OPC UA server address space. This option allows selecting only subsets of the OPC UA address by explicitly listing one or more NodeIds which are used to start the initial browse.<br><br>Examples:<br>`"ns=5;s=85/0:Simulation"`<br>`"ns=3;s=DataItems"`<br><br>If not specified, it means that the whole address space will be browsed.|
| **IncomingTimestamp**	| Optional | `string` | No | Specifies whether the incoming timestamp is taken from the source, from the OPC UA server, or should be created by the OPC UA adapter instance. **Source** - Default and recommended setting. The timestamp is taken from the source timestamp field. The source is what provides data for the item to the OPC UA server, such as a field device. **Server** - In case the OPC UA item has an invalid source timestamp field, the Server timestamp can be used. **Adapter** - The OPC UA adapter generates a timestamp for the item upon receiving it from the OPC UA server.|
| **StreamPrefix** | Optional | `string` | Yes | Specifies what prefix is used for Stream IDs and names. Naming convention is StreamPrefixNodeId and StreamPrefixName. **Note:** An empty string means no prefix will be added to the Stream IDs and names. Null value means ComponentID followed by dot character will be added to the stream IDs and names (for example, OpcUa1.NodeId).|
| **ApplyPrefixToStreamId** | Optional | `boolean` | No | Parameter applied to all data items collected from the data source that have custom stream ID configured. If set to `true`, the adapter will apply the **StreamPrefix** property to all streams with custom ID configured. The property does not affect any streams with default ID configured.|


## OPC UA data source example

The following is an example of valid OPC UA data source configuration:

```json
{
    "EndpointUrl": "opc.tcp://<IP-Address>:<Port>/<TestOPCUAServer>",
    "UseSecureConnection": true,
    "UserName": null,
    "Password": null,
    "RootNodeIds": null,
    "IncomingTimestamp": "Source",
    "StreamPrefix": null,
    "ApplyPrefixToStreamId": false
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSource  | GET | Retrieves the OPC UA data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | PUT | Configures or updates the OPC UA data source configuration |
| api/v1/configuration/_ComponentId_/DataSource | DELETE | Deletes the OPC UA data source configuration |

**Note:** Replace _ComponentId_ with the Id of your OPC UA component, for example OpcUa1.
