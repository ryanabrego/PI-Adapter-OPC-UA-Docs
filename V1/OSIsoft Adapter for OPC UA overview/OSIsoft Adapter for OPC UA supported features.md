---
uid: OSIsoftAdapterForOPCUASupportedFeatures
---

# OSIsoft Adapter for OPC UA supported features

This topic provides an overview of the features supported by OPC UA adapter, focusing on data types and export operation.

## Data types

The following table lists OPC UA variable types that the OPC UA adapter supports data collection from and types of streams that are going to be created.

| OPC UA data type | Stream data type |
|------------------|------------------|
| Boolean          | Boolean          |
| Byte             | Int16            |
| SByte            | Int16            |
| Int16            | Int16            |
| UInt16           | UInt16           |
| Int32            | Int32            |
| UInt32           | UInt32           |
| Int64            | Int64            |
| UInt64           | UInt64           |
| Float            | Float32          |
| Double           | Float64          |
| DateTime         | DateTime         |
| String           | String           |

## Export operation

The OPC UA adapter is able to export available OPC UA dynamic variables by browsing the OPC UA hierarchies or sub-hierarchies.

### Limit browsing

Browsing can be limited by specifying a comma-separated collection of nodeIds in data source configuration (RootNodeIds) which are treated as a roots from where the adapter starts the browse operation. For more information, see [OSIsoft Adapter for OPC UA data source configuration](xref:OSIsoftAdapterForOPCUADataSourceConfiguration).

The adapter triggers an export operation after a successful connection to the OPC UA server when the data selection file does not exist in configuration directory. The exported data selection JSON file can be copied from the directory or retrieved using a REST API call.

### Manually create data selection file

The data selection file can be created manually in order to avoid a potentially long and expensive browse operation. It can be configured the data source is configured or both pushed in one configuration call together. For more information, see [OSIsoft Adapter for OPC UA data selection configuration](xref:OSIsoftAdapterForOPCUADataSelectionConfiguration#generate-default-opc-ua-data-selection-configuration-file).
