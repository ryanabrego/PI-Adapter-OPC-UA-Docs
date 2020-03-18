---
uid: OSIsoftAdapterForOPCUADataSelectionConfiguration
---

# OSIsoft Adapter for OPC UA data selection configuration

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

When you add a data source, the adapter browses the entire OPC UA server address space and exports the available OPC UA variables into a JSON file for data selection. Comma-separated collection of nodeIds (RootNodeIds) serve as filters to browse only a subset of the OPC UA server. Data is collected automatically based upon user demands. OPC UA data from OPC UA variables is read through subscriptions (unsolicited reads).

You can decide to have the data selection configuration file generated automatically or you can create it manually yourself.

## Generate default OPC UA data selection configuration file

A default OPC UA data selection file will be created if there is no OPC UA data selection configuration although a valid data source exists.

**Note:** To avoid possible time and resource expensive browse operations due to reasons described previously, OSIsoft recommends that you manually create a data selection file instead of automatically creating the default data selection file. For more information, see [Configure OPC UA data selection](#configure-opc-ua-data-selection).

Complete the following procedure for this default data selection file to be generated:

1. Add an OPC UA adapter with a unique ComponentId. For more information, see [System components configuration](xref:SystemComponentsConfiguration).
  
2. Configure a valid OPC UA data source. For more information, see [OSIsoft Adapter for OPC UA data source configuration](xref:OSIsoftAdapterForOPCUADataSourceConfiguration).

   Once you complete these steps, a default OPC UA data selection configuration file will be generated in the configuration directory for the corresponding platform.
  
   The following are example locations of the file created. In this example, it is assumed that the ComponentId of the OPC UA component is OpcUa1:

   Windows: *%programdata%\OSIsoft\Adapters\OpcUa\OpcUa\Configuration\OpcUa1_DataSelection.json*
  
   Linux: */usr/share/OSIsoft/Adapters/OpcUa/OpcUa/Configuration/OpcUa1_DataSelection.json*

3. Copy the file to a different directory.
    - For content structure, see [OPC UA data selection example](#opc-ua-data-selection-example).

4. Using any text editor, change the value of any **Selected** key from `false` to `true` in the file.

   Once the configuration is updated, the adapter will subscribe to data for all items that are set to *Selected=true*.

5. In the same directory where you edited the file, run the following curl command:

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

      ```bash
      curl -i -d "@OpcUa1_DataSelection.json" -H "Content-Type: application/json" -X PUT      "http://localhost:5590/api/v1/configuration/OpcUa1/Dataselection"
      ```

## Configure OPC UA data selection

**Note:** You cannot modify OPC UA data selection configurations manually. You must use the REST endpoints to add or edit the configuration.

Complete the following procedure to configure the OPC UA data selection:

1. Using any text editor, create a file that contains an OPC UA data selection in JSON form.
    - For content structure, see [OPC UA data selection example](#opc-ua-data-selection-example).
    - For a table of all available parameters, see [OPC UA data selection](#opc-ua-data-selection-parameters).
2. Save the file, for example as _DataSelection.config.json_.
3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to execute a POST command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<adapterId>/DataSelection/`

    Example using curl (run this command from the same directory where the file is located):

    **Note:** The following example uses OpcUa1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    ```bash
    curl -d "@DataSelection.config.json" -H "Content-Type: application/json" -X POST "http://localhost:5590/api/v1/configuration/OpcUa1/DataSelection"
    ```

## OPC UA data selection schema

The full schema definition for the OPC UA data selection configuration is in the _OpcUa_DataSelection_schema.json_ here:

Windows: *%ProgramFiles%\OSIsoft\Adapters\OpcUa\Schemas*

Linux: */opt/OSIsoft/Adapters/OpcUa/Schemas*

## OPC UA data selection parameters

The following parameters are available for configuring an OPC UA data selection:

| Parameter     | Required | Type | Description |
|---------------|----------|------|-------------|
| **Selected** | Optional | `boolean` | Use this field to select or clear a measurement. To select an item, set to true. To remove an item, leave the field empty or set to false.  If not configured, the default value is true.|
| **Name**      | Required | `string` | The optional friendly name of the data item collected from the data source. |
| **NodeId**    | Required | `string` | The NodeId of the variable.<br><br>Examples<br>`"ns=5;AString"`<br>`"ns=2;i=203"`<br>`"ns=<NamespaceIndex>;<IdentifierType>=<Identifer>"` |
| **StreamID** | Optional | `string` | The custom stream ID used to create the streams. If not specified, the adapter will generate a default stream ID based on the measurement configuration. A properly configured custom stream ID follows these rules:<br><br>Is not case-sensitive.<br>Can contain spaces.<br>Cannot start with two underscores ("__").<br>Can contain a maximum of 100 characters.<br>Cannot use the following characters: / : ? # [ ] @ ! $ & ' ( ) \ * + , ; = % < > &#124;<br>Cannot start or end with a period.<br>Cannot contain consecutive periods.<br>Cannot consist of only periods.

## OPC UA data selection example

The following are examples of valid OPC UA data selection configurations:

**Minimum data selection configuration**:

```json
[
 {
    "Name": "Random1",
    "NodeId": "ns=5;s=Random1"
  },
  {
    "Name": "Sawtooth1",
    "NodeId": "ns=5;s=Sawtooth1"
  },
  {
    "Name": "Sinusoid1",
    "NodeId": "ns=5;s=Sinusoid1"
  }
]
```

**Maximum data selection configuration**:

```json
[
 {
    "Selected": true,
    "Name": "Random1",
    "NodeId": "ns=5;s=Random1",
    "StreamId": "CustomStreamName"
  },
  {
    "Selected": false,
    "Name": "Sawtooth1",
    "NodeId": "ns=5;s=Sawtooth1",
    "StreamId": null
  },
  {
    "Selected": true,
    "Name": "Sinusoid1",
    "NodeId": "ns=5;s=Sinusoid1",
    "StreamId": null
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSelection  | GET | Retrieves the OPC UA data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection  | PUT | Configures or updates the OPC UA data selection configuration |
| api/v1/configuration/_ComponentId_/DataSelection | DELETE | Deletes the OPC UA data selection configuration |

**Note:** Replace _ComponentId_ with the Id of your OPC UA component, for example OpcUa1.
