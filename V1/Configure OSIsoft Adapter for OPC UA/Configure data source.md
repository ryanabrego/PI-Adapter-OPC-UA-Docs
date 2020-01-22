---
uid: ConfigureDataSourceOPCUA
---

# Configure data source

In order to use the OPC UA adapter, you must configure the data source from which it will receive data.

**Note:** You cannot modify OPC UA data source configurations manually. You must use the REST endpoints to add or edit the configuration.

Complete the following to configure the OPC UA data source:

1. Using any text editor, create a file that contains an OPC UA data source in JSON form.
    - For content structure, see [OPC UA data source example](#opc-ua-data-source-example).
    - For a table of all available parameters, see [Parameters for OPC UA data source](#parameters-for-opc-ua-data-source).
2. Save the file as _DataSource.config.json_.
3. Use any [Configuration tool](xref:ConfigurationTools) capable of making HTTP requests to execute a POST command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<EDS adapterId>/DataSource/`. 

The following example shows the HTTPS request using curl (run this command from the same directory where the file is located):

**Note:** During installation, you can add a single OPC UA adapter named OpcUa1. The following example uses this component name.

```bash
curl -v -d "@DataSource.config.json" -H "Content-Type: application/json" "http://localhost:5590/api/v1/configuration/OpcUa1/DataSource"
```

## OPC UA data source schema

The following table shows the basic behavior of the _OpcUa_DataSource_schema.json_ file.

| Abstract            | Extensible | Status       | Identifiable | Custom properties | Additional properties |
| ------------------- | ---------- | ------------ | ------------ | ----------------- | --------------------- |
| Can be instantiated | Yes        | Experimental | No           | Forbidden         | Forbidden             |

## Parameters for OPC UA data source

The following parameters can be used to configure an OPC UA data source:

| Parameter | Required | Type | Nullable | Description |
|-----------|----------|------|----------|-------------|
| **EndpointURL** | Required | `string` | Yes | The endpoint URL of the OPC UA server. The following is an example of the URL format: opc.tcp://OPCServerHost:Port/OpcUa/SimulationServer<br><br>**Note:** If you change the EndpointURL on a configured OPC UA adapter that has ComponentID_DataSelection.json file exported, you need to remove the _ComponentID_DataSelection.json_ file from the configuration directory to trigger a new browse (export).|
| **UseSecureConnection**|Optional | `Boolean` | No | When set to true, the OPC UA adapter connects to a secure endpoint using OPC UA certificate exchange operation. The default is true. When set to false, the OPC UA adapter connects to an unsecured endpoint of the server and certificate exchange operation is not required.<br><br>**Note:** OSIsoft recommends setting this option to false for testing purposes only.|
| **UserName** | Optional | `string` | Yes | User name for accessing the OPC UA server. |
| **Password** | Optional | `string` | Yes | Password for accessing the OPC UA server.<br><br>**Note:** OSIsoft recommends using REST to configure the data source when the password must be specified.|
| **RootNodeIds** | Optional | `string` | Yes |List of comma-separated NodeIds of those objects from which the OPC UA adapter browses the OPC UA server address space. This option allows selecting only subsets of the OPC UA address by explicitly listing one or more NodeIds which are used to start the initial browse. For example: ns=5;s=85/0:Simulation, ns=3;s=DataItems. If not specified, it means that the whole address space will be browsed.|
| **IncomingTimestamp**	| Optional | `string` | No | Specifies whether the incoming timestamp is taken from the source, from the OPC UA server, or should be created by the OPC UA adapter instance. **Source** - Default and recommended setting. The timestamp is taken from the source timestamp field. The source is what provides data for the item to the OPC UA server, such as a field device. **Server** - In case the OPC UA item has an invalid source timestamp field, the Server timestamp can be used. **Connector** - The OPC UA adapter generates a timestamp for the item upon receiving it from the OPC UA server.|
| **StreamIdPrefix** | Optional | `string` | Yes | Specifies what prefix is used for Stream IDs. Naming convention is StreamIdPrefixNodeId. **Note:** An empty string means no prefix will be added to the Stream IDs. Null value means ComponentID followed by dot character will be added to the stream IDs (for example, OpcUa1.NodeId).|
| **ApplyPrefixToStreamId** | Optional          | `boolean` | No | Parameter applied to all data items collected from the data source that have custom stream ID configured. If configured, the adapter will apply the StreamIdPrefix property to all the streams with custom ID configured. The property does not affect any streams with default ID configured|


## OPC UA data source example

The following is an example of valid OPC UA data source configuration:

```json
{
    "EndpointUrl": "opc.tcp://IP-Address/TestOPCUAServer",
    "UseSecureConnection": true,
    "UserName": null,
    "Password": null,
    "RootNodeIds": null,
    "IncomingTimestamp": "Source",
    "StreamIdPrefix": null
}
```
