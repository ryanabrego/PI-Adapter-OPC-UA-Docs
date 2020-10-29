---
uid: PIAdapterForOPCUAPrinciplesOfOperation1-2
---

# PI Adapter for OPC UA principles of operation

This adapter's operations focus on data collection and stream creation.

## Adapter configuration

For the OPC UA adapter to start data collection, configure the following:

- Data source: Provide the data source from which the adapter should collect data.
- Data selection: Select the OPC UA items to which the adapter should subscribe for data.
- Logging: Set up the logging attributes to manage the adapter logging behavior.

For more information, see [PI Adapter for OPC UA data source configuration](xref:PIAdapterForOPCUADataSourceConfiguration1-2) and [PI Adapter for OPC UA data selection configuration](xref:PIAdapterForOPCUADataSelectionConfiguration1-2).

## Connection

The OPC UA adapter uses the binary opc.tcp protocol to communicate with the OPC UA servers. As part of the OPC UA server and client establishing a secured connection, each one sends its X.509-type certificate to the other for verification. Upon verification of the certificates, the server and client establishes a secured connection.

For more information on secure connections, see [PI Adapter for OPC UA security configuration](xref:PIAdapterForOPCUASecurityConfiguration1-2).

## Data collection

The OPC UA adapter collects time-series data from selected OPC UA dynamic variables through OPC UA subscriptions (unsolicited reads). The adapter supports the Data Access (DA) part of OPC UA specification. For more information, see [Unified Architecture (https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-8-data-access)](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-8-data-access).

### Data types

The following table lists OPC UA variable types that the adapter collects data from and types of streams that will be created.

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

## Stream creation

The OPC UA adapter creates a stream with two properties for each selected OPC UA item. The properties are described in the following table:

| Property name | Data type | Description |
|---------------|-----------|-------------|
| Timestamp     | DateTime  | Timestamp of the given OPC UA item value update. |
| Value         | Based on type of incoming OPC UA value | Value of the given OPC UA item update, which includes multiple properties in addition to the data value.<br><br>**Note:**<br>For OPC UA items that support EURange, the additional **Minimum**/**Maximum** properties in OCS and the **Zero**/**Span** properties in PI Web API are populated.<br>For OPC UA items that support EngineeringUnits, such as AnalogItem, the additional **UOM** property in OCS and the **Eng Units** property in PI Web API are populated.  |

The OPC UA adapter sends metadata with each stream it creates. Metadata common for every adapter type are

- **ComponentId**: Specifies the data source, for example _OpcUa1_
- **ComponentType**: Specifies the type of adapter, for example _OpcUa_

Metadata specific to the OPC UA adapter are

- **BrowseName**: The browse name as provided by the OPC UA server
- **SourceId**: The NodeId provided by the OPC UA server

**Note:** A configured metadata level allows you to set the amount of metadata for the adapter. Specify the metadata level in [General configuration](xref:GeneralConfiguration1-3). For the OPC UA adapter, the following metadata is sent for the individual level:

- `None`: No metadata
- `Low`: AdapterType (ComponentType) and DataSource (ComponentId)
- `Medium`: AdapterType (ComponentType), DataSource (ComponentId), BrowseName, and DisplayName
- `High`: AdapterType (ComponentType), DataSource (ComponentId), BrowseName, DisplayName, and SourceId

Each stream created by  the adapter for a given OPC UA item has a unique identifier (Stream ID). If you specify a custom stream ID for the OPC UA item in data selection configuration, the OPC UA adapter uses that stream ID to create the stream. Otherwise, the adapter constructs the stream ID using the following format, which is constructed from the OPC UA item node ID:

```code
<AdapterComponentID>.<NamespaceIndex>.<Identifier>
```
NamespaceIndex refers to the number specified in the `ns` keyword in the **RootNodeIds** parameter of the data source configuration. For more information, see [PI Adapter for OPC UA data source configuration](xref:PIAdapterForOPCUADataSourceConfiguration1-2#opc-ua-data-source-parameters).

**Note:** The naming convention is affected by StreamPrefix and DefaultStreamIdPattern settings in data source configuration.
