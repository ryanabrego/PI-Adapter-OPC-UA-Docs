---
uid: OSIsoftAdapterForOPCUASupportedFeatures
---

# OSIsoft Adapter for OPC UA supported features

## Data types

The following table lists OPC UA variable types that the OPC UA EDS adapter supports data collection from and types of streams that are going to be created in Edge Data Store.

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

The OPC UA EDS adapter is able to export available OPC UA dynamic variables by browsing the OPC UA hierarchies or sub-hierarchies. You can limit browsing by specifying a comma-separated collection of nodeIds in data source configuration (RootNodeIds) which are treated as a roots from where the adapter starts the browse operation. The adapter triggers an export operation after a successful connection to the OPC UA server when the data selection file does not exist in configuration directory. You can copy the exported data selection JSON file from the directory or retrieve it using a REST API call.

You can also create the data selection file manually in order to avoid a potentially long and expensive browse operation. You can configure it before you configure the data source or push both in one configuration call together.
