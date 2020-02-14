---
uid: OSIsoftAdapterForOPCUAOverview
---

# OSIsoft Adapter for OPC UA overview

The OSIsoft Adapter for OPC UA is a data-collection component that transfers time-series data from source devices to OSIsoft OMF endpoints in OSIsoft Cloud Services or PI Servers. OPC Unified Architecture (OPC UA) is an open standard, machine-to-machine communication protocol for industrial automation developed by the OPC Foundation. The OPC UA adapter can connect to any device that uses the OPC UA communication protocol.

The OSIsoft OPC UA Adapter is installed with a download kit obtained from the OSIsoft Customer Portal and works on devices running either Windows or Linux operating systems. 

All functions of the adapter are configured using JSON files. For data ingress, an adapter system component must be defined for each device to which the adapter will connect. Each adapter system component is then configured with the connection information for the device, the data to collect, and the security for the connection. For data egress, configurations are needed to specify the destination for the data and the security for the outgoing connection. Additional configurations are available to egress health and diagnostics data, add data buffering to protect against data loss, and record logging information for troubleshooting. 

Once the adapter is configured and sending data, use administration functions to manage the adapter or individual ingress components of the adapter. Use health and diagnostics functions to monitor the status of connected devices, adapter system functions, the number of active data streams, the rate of data ingress, the rate of errors, and the rate of data egress.

The EdgeCmd utility is an OSIsoft proprietary command line tool that is used to configure and administer an adapter on both Linux and Windows operating systems. It is installed separately from the adapter and provides an alternative to REST tools.

