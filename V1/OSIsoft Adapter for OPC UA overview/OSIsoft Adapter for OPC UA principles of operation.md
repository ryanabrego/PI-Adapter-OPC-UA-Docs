---
uid: OSIsoftAdapterForOPCUAPrinciplesOfOperation
---

# OSIsoft Adapter for OPC UA principles of operation

This adapter's operations focus on data collection and stream creation.

## Adapter configuration

For the OPC UA adapter to start data collection, configure the following:

- Data source: Provide the data source from which the adapter should collect data.
- Data selection: Perform selection of OPC UA items to which the adapter should subscribe for data.
- Logging: Set up the logging attributes to manage the adapter logging behavior.

For more information, see [OSIsoft Adapter for OPC UA data source configuration](xref:OSIsoftAdapterForOPCUADataSourceConfiguration) and [OSIsoft Adapter for OPC UA data selection configuration](xref:OSIsoftAdapterForOPCUADataSelectionConfiguration).

## Connection

The OPC UA adapter uses the binary opc.tcp protocol to communicate with the OPC UA servers. When a secured connection is enabled, the X.509-type client and server certificates are exchanged and verified and the connection between the OPC UA adapter and the configured OPC UA server is established.

For more information on secure connections, see [OSIsoft Adapter for OPC UA security configuration](xref:OSIsoftAdapterForOPCUASecurityConfiguration).

## Data collection

The OPC UA adapter collects time-series data from selected OPC UA dynamic variables through OPC UA subscriptions (unsolicited reads). The adapter supports the Data Access (DA) part of OPC UA specification. For more information, see [Unified Architecture (https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-8-data-access)](https://opcfoundation.org/developer-tools/specifications-unified-architecture/part-8-data-access).

### Data types

The following table lists OPC UA variable types that the adapter supports data collection from and types of streams that will be created.

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

The OPC UA adapter creates types upon receiving the value update for a stream. One stream is created for every selected OPC UA item in the data selection configuration.

### Streams by OPC UA adapter

The OPC UA adapter creates a stream with two properties for each selected OPC UA item. The properties are described in the following table:

| Property name | Data type | Description |
|---------------|-----------|-------------|
| Timestamp     | DateTime  | Timestamp of the given OPC UA item value update. |
| Value         | Based on type of incoming OPC UA value | Value of the given OPC UA item update. |

Certain metadata are sent with each stream created. Metadata common for every adapter type are

- **ComponentId**: Specifies the type of adapter, for example _OpcUa_
- **ComponentType**: Specifies the data source, for example _OpcUa1_

Metadata specific to the OPC UA adapter are

- **BrowseName**: The browse name as provided by the OPC UA server
- **SourceId**: The NodeId provided by the OPC UA server

Each stream created by  the adapter for a given OPC UA item has a unique identifier (Stream ID). If a custom stream ID is specified for the OPC UA item in data selection configuration, the OPC UA adapter uses that stream ID to create the stream. Otherwise, the adapter constructs the stream ID using the following format, which is constructed from the OPC UA item node ID:

```code
<Adapter Component ID>.<NamespaceIndex>.<Identifier>
```

**Note:** The naming convention is affected by StreamPrefix and ApplyPrefixToStreamID settings in data source configuration. For more information, see [OSIsoft Adapter for OPC UA data source configuration](xref:OSIsoftAdapterForOPCUADataSourceConfiguration).
