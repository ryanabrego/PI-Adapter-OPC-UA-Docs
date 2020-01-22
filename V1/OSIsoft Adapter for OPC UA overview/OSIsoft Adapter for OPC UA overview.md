---
uid: OSIsoftAdapterForOPCUAOverview
---

# OSIsoft Adapter for OPC UA overview

OPC UA is an open standard, which ensures interoperability, security, and reliability of industrial automation devices and systems. OPC UA is recognized as one of the key communication and data modeling technologies of Industry 4.0, due to the fact that it works with many software platforms, and is completely scalable and flexible.

The OPC UA adapter transfers time-series data from OPC UA devices into Edge Data Store.

You can add a single OPC UA adapter during installation. To install multiple OPC UA adapters, see [Edge Data Store Configuration](xref:EdgeDataStoreConfiguration) for adding a new component to Edge Data Store.

As with other adapters, the OPC UA adapter is configured with data source and data selection JSON documents. The data source configurations are identical with other adapters, but OPC UA supports an option to generate a data selection file template that you can manually edit and use for subsequent configuration. For details, see [OPC UA data selection configuration](xref:OPCUADataSelectionConfiguration). Once you create a template file, you can reuse it on both Linux and Windows without changes.

For more information on how to configure logging for the OPC UA adapter, see [Logging configuration](xref:LoggingConfiguration).
