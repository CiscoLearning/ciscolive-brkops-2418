# BRKOPS-2418 | Cisco Live US 2025

This repository will serve as a single source of information and follow-up for BRKOPS-2418 presented at CLUS2025.  While not completely canonical, the resources will include:

- reference links to documentation for all tools deployed
- sample configurations of required add-ons to Splunk
- dashboard layouts and panels to be imported into a Splunk instance

The goal of this repository is to be a living document, rather than a snapshot of work in time.  If deploying any of the technologies covered in this session, it may be beneficial to bookmark this page as a continuing reference.

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
