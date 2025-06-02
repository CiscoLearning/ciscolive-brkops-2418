# Installation and configuration for Netflow Data Collection

In order for Splunk to *natively* handle Netflow data, an additional piece of software will need to be running within the Splunk collection environment, both as technical add-ons for viewing and decoding Netflow packet data, as well as forwarders that send the data to the databases.
The forwarders send the data to the Splunk indexers using the **HTTP Event Collector (HEC)**.  This allows for security controls and firewalls to segment security zones and allow collections of Netflow data without opening up firewall ports for network data.  Additionally, since Netflow is transmitted in clear-text, the translation to HTTPS using the Splunk HEC, data is protected in flight.

Unlike other collection methods, Netflow data is not collected using a containerized application, but is installed as a technical add-on (TA) within Splunk.  The TA is installed on the Splunk indexers and forwarders, and the data is collected using the Splunk Universal Forwarder (UF) or Heavy Forwarder (HF).  The data is then sent to the Splunk indexers for indexing and searching.

This guide will cover the installation of the Splunk TAs required for Netflow data collection, as well as the configuration of the Splunk indexers and forwarders to collect and send the data.

## TA Downloads and Installation

The Splunk TAs required for Netflow data collection can be downloaded from the Splunkbase website.  The following TAs are required: