---
uid: OPCUAConfiguration
---

# Configuration

PI Adapter for OPC UA enables you to configure data source and data selection. The adapter also provides the ability to configure security and to generate a data selection file instead of manual configuration.

The examples in the configuration topics use `curl`, a commonly available tool on both Windows and Linux. The adapter can be configured with any programming language or tool that supports making REST calls, or with the EdgeCmd utility. For more information, see the [EdgeCmd utility documentation (https://osisoft.github.io/Edgecmd-Docs/V1.1/edgecmd-utility.html)](https://osisoft.github.io/Edgecmd-Docs/V1.1/edgecmd-utility.html). To validate successful configurations, you can perform data retrieval (GET commands) using a browser, if available on your device.

For more information on PI adapter configuration tools, see [Configuration tools](xref:ConfigurationTools).

## Quick start

Complete the following steps to establish a data flow from an OPC UA data source device to a data endpoint.

1. Configure one or several OPC UA system components.<br>See [System components configuration](xref:SystemComponentsConfiguration#add-a-system-component).

2. Configure an OPC UA data source for each OPC UA device.<br>See [PI Adapter for OPC UA data source configuration](xref:PIAdapterForOPCUADataSourceConfiguration#configure-opc-ua-data-source).

3. Configure an OPC UA data selection for each OPC UA data source.<br>See [PI Adapter for OPC UA data selection configuration](xref:PIAdapterForOPCUADataSelectionConfiguration#configure-opc-ua-data-selection).<br><br>**Note:** If you do not configure data selection, a default OPC UA data selection file will be created if the OPC UA data source is valid.

4. Configure one or several egress endpoints.<br>See [Egress endpoints configuration](xref:EgressEndpointsConfiguration).
