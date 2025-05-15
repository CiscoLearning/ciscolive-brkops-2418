# BRKOPS-2418 | Cisco Live US 2025

This repository will serve as a single source of information and follow-up for BRKOPS-2418 presented at CLUS2025.  While not completely canonical, the resources will include:

- reference links to documentation for all tools deployed
- sample configurations of required add-ons to Splunk
- dashboard layouts and panels to be imported into a Splunk instance

The goal of this repository is to be a living document, rather than a snapshot of work in time.  If deploying any of the technologies covered in this session, it may be beneficial to bookmark this page as a continuing reference.

## Installation

To keep this landing README to a reasonable size, the respective installation guides for the specific network telemetry protocol are listed below, with separate guides.

- [**Syslog**](./installations/SYSLOG.md)
- [**NetFlow**](./installations/NETFLOW.md)
- [**SNMP**](./installations/SNMP.md)
- [**Model Driven Telemetry**](./installations/MDT.md) (*coming soon*)

## Specific Notes

### NetFlow

- The Splunk Technical Add-ons (TAs) indicate that they support NetFlow v5/v9/IPFIX.  However, in my experience, while the data will be ingested if versions above 5 are used, the byte count will show as '0' in every flow event.  This affects any chart or visualization using these versions.  Because of this, v5 has been tested and validated to work.

### SNMP

- `sc4snmp` is supported for both `docker compose` and Kubernetes-based installations.  For Kubernetes-based installs, `microk8s` is documented within the official `sc4snmp` documentation, however it can be installed on any Kubernetes installation that supports Helm v3 charts.
- `sc4snmp` polling data (interface statistics, etc) require

## Links

### SNMP Polling and Traps

- [Splunk Connect for SNMP (sc4snmp)](https://splunk.github.io/splunk-connect-for-snmp/develop/)
- [sc4snmp Installation using Helm and Microk8s](https://splunk.github.io/splunk-connect-for-snmp/develop/microk8s/sc4snmp-installation/)
- [sc4snmp Github](https://github.com/splunk/splunk-connect-for-snmp/tree/main)
- [Cisco SNMP Counter FAQ](https://www.cisco.com/c/en/us/support/docs/ip/simple-network-management-protocol-snmp/26007-faq-snmpcounter.html)

### NetFlow

- [Splunk Stream Installation and Configuration Guide](https://docs.splunk.com/Documentation/StreamApp/7.2.0/DeployStreamApp/InstallSplunkAppforStream)
- [Configure Splunk Stream for Netflow Collection](https://docs.splunk.com/Documentation/StreamApp/7.1.2/DeployStreamApp/ConfigureFlowcollector)
- [Use Splunk Stream to Ingest Netflow and IPFIX Data](https://docs.splunk.com/Documentation/StreamApp/8.1.3/DeployStreamApp/UseStreamtoingestNetflowandIPFIXdata)
- [Splunking NetFlow with Splunk Stream - Part 1: Getting NetFlow data into Splunk](https://www.splunk.com/en_us/blog/tips-and-tricks/splunking-netflow-with-splunk-stream-part-1-getting-netflow-data-into-splunk.html?locale=en_us)
- [Splunking Netflow with Splunk Stream - Part 2: Basic Netflow Analytics](https://www.splunk.com/en_us/blog/tips-and-tricks/splunking-netflow-with-splunk-stream-part-2-basic-netflow-analytics.html)
- [Netflow Sample Dashboards](https://github.com/lucas-alados/netflow_sample_dashboards)
