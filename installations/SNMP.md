# Installation and Configuration for SNMP Data Ingestion

In order for Splunk to *natively* handle SNMP data, an additional piece of software will need to be running somewhere within the environment.  This software is a series of containers that receive SNMP traps and use a Python library to perform SNMP querying and walking.  Multiple copies of this software can run at different locations within the network and then forward the traffic to Splunk for data ingestion over the **HTTP Event Collector (HEC)**.  This allows for security controls and firewalls to segment security zones and allow collections of SNMP data without opening up firewall ports for network data.  Additionally, since SNMP is transmitted in clear-text, the translation to HTTPS using the Splunk HEC, data is protected in flight.

This guide will cover installation of Splunk Connect for SNMP (SC4SNMP) using `microk8s`, rather than using `docker compose`.  Reference the SC4SNMP project page (link given in [README.md](../README.md)) if you desire to use the `docker compose` method.  The structure and files required for the `docker compose` method are provided in the `configurations/splunk/sc4snmp/docker-compose` directory.

## `microk8s`

`microk8s` is a lightweight Kubernetes installation available from Canonical (the creators of Ubuntu).  `microk8s` is packaged using `snap` for easy installation, however, its available for other operating systems (and architectures) and documentation for alternative installations can be found [here](https://microk8s.io/docs/install-alternatives).

### Installation

> Note: This guide assumes you are using an Ubuntu-based workstation, and have `snap` installed.  If you are using a different operating system, please refer to the [microk8s documentation](https://microk8s.io/docs/install-alternatives) for installation instructions.

1. Install `microk8s` using `snap`:

   ```bash
   sudo snap install microk8s --classic
   ```

2. Add your user to the `microk8s` group:

   ```bash
   sudo usermod -a -G microk8s $USER
   sudo chown -f -R $USER ~/.kube
   su - $USER
   ```

3. Enable the required `microk8s` add-ons:

   ```bash
   sudo systemctl enable iscsid
   microk8s enable helm3
   microk8s enable hostpath-storage
   microk8s enable rbac
   microk8s enable metrics-server
   ```

4. Enable DNS resolution for `microk8s`:

   ```bash
   microk8s enable dns:208.67.222.222,208.67.220.220
   ```

5. Install MetalLB for load balancing:

   ```bash
   microk8s enable metallb
   ```

6. Ensure that Microk8s is running and ready

    ```bash
    microk8s status --wait-ready
    ```

## Splunk Indexes, Collectors

In order for data to be ingested into Splunk, you will need to create two indexes for the SNMP data.  The first index is for the SNMP traps, and the second index is for the SNMP polling data. Additional indexes can be created for telemetry data for the `sc4snmp` application itself, but these are not required for basic SNMP data ingestion.

The indexes to be created are:

- `netmetrics` - a **metrics** index for all SNMP polling data
- `netops` - an ***events** index for all SNMP traps

> Note: the names of the indexes can be changed and edited within the `values.yaml` file, but pay attention to the types of indexes used.  Metrics indexes are similar to a TSDB and use different queries to access the data contained within them.  By default, when creating a new index in Splunk, it will be created as an **events** index.

Additionally, you will need to create a **HTTP Event Collector (HEC)** in Splunk to allow the `sc4snmp` application to send data to Splunk.  When creating the HEC, you will need to capture the token value, as this will be required in the `values.yaml` file under the `token:` key.  BOth indexes will use the same HEC token, so you only need to create one HEC.


## `sc4snmp`

Once `microk8s` is installed and running, you can deploy the Splunk Connect for SNMP (SC4SNMP) application.  The following steps will guide you through the deployment process.  You will need to use the files provided in the `configurations/splunk/sc4snmp/microk8s` directory, but these can be edited to suit your environment.  **At a minimum**, you will need to edit the `values.yaml` file to include your Splunk HEC token, URL, and port number of the HEC, as well as the IP addresses of the devices you want to monitor via SNMP polling.

1. Add the `sc4snmp` Helm repository and update the local Helm repository cache:

   ```bash
   microk8s helm3 repo add sc4snmp https://splunk.github.io/splunk-connect-for-snmp
   microk8s helm3 repo update
   ```

2. Install `sc4snmp` using the edited `values.yaml` file

   ```bash
   microk8s helm3 install snmp -f values.yaml splunk-connect-for-snmp/splunk-connect-for-snmp --namespace=sc4snmp --create-namespace
   ```

3. Verify that the `sc4snmp` application is running:

   ```bash
   microk8s kubectl get pods -n sc4snmp
   ```

4. Verify that Splunk is receiving data from the `sc4snmp` application by searching for data in the `netmetrics` and `netops` indexes:

   ```spl
   index="netops" sourcetype="sc4snmp:event"
   ```

   ```spl
   | mpreview index="netmetrics" | search sourcetype="sc4snmp:metric"
   ```

## Monitoring and Dashboards

Once data is being ingested into Splunk, you can begin to create dashboards and alerts based on the SNMP data.  The `sc4snmp` application provides a number of pre-built dashboards and alerts that can be used to monitor your network devices.  Additionally, you can create custom dashboards and alerts based on the data being ingested.  The prepackaged dashboards are available [here](https://github.com/splunk/observability-content-contrib/tree/main/dashboards-and-dashboard-groups/SC4SNMP) and a custom dashboard is also included in this repository at `configurations/splunk/sc4snmp/snmp_dashboard.json`.