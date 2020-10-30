---
uid: TroubleshootPIAdapterForOPCUA
---

# Troubleshoot PI Adapter for OPC UA

If you encounter issues with PI Adapter for OPC UA, you can attempt to troubleshoot on your own before contacting OSIsoft Technical Support. Start by checking the adapter's configuration, connectivity, and logs. The following sections provide more details.

## Check configurations

1. In the [data source configuration](xref:PIAdapterForOPCUADataSourceConfiguration), verify the accuracy of the configured **EndpointURL** and **RootNodeIds**.
2. In the [data selection configuration](xref:PIAdapterForOPCUADataSelectionConfiguration), verify the accuracy of each configured data selection item.

    1. For the **NodeId**, verify that the referenced root node exists in the data source configuration.
    2. If you configured **DataFilterId**, verify that the referenced data filter exists.

3. In the [egress endpoints configuration](xref:EgressEndpointsConfiguration), verify the accuracy of each configured endpoint's **Endpoint** property and credentials (For a PI server or EDS endpoint **UserName** and **Password**, for an OCS endpoint **ClientId** and **ClientSecret**).

## Check connectivity

1. Verify that there are active connections to the data source and egress endpoints.
2. If you configured a health endpoint, use it to determine the status of the adapter.<br>For more information, see [Health and diagnostics](xref:HealthAndDiagnostics).

## Check logs

1. To isolate the issue, check the adapter and endpoint logs.
2. Optional: Change the log level of the adapter to receive more information and context.<br>For more information, see [Logging configuration](xref:LoggingConfiguration).
