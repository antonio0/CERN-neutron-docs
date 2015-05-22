# CERN Network Restrictions

## Network Clusters
The CERN network is divided into several clusters, each one divided in multiple IP services (independent L3 networks).
It is necessary to introduce a representation of these network clusters in Neutron. An extension named *SubnetCluster* has been developed to allow grouping of subnets called clusters.
A cluster object has an ID, Name and a list of associated subnets. The Neutron CLI has been modified to offer the following commands:
- *cluster-list*
- *cluster-show*
- *cluster-update*
- *cluster-delete*
- *cluster-insert-subnet*
- *cluster-remove-subnet*

## IP Services
Each compute node has got an associated VM cluster, which is composed of many secondary IP services containing the IP addresses a VM interface on that node can have.
Therefore, this restriction means that Neutron's seemingly-random way of choosing an IP address on a network for a new port is not suitable for our network.

The solution that has been implemented makes the compute service specify a subnet on the port request (already supported on Neutron side). The subnet needs to be determined based on the compute node that is requesting the port. In order to assist the compute service in the subnet selection, an extension has been developed.
The *HostRestrictions* extension exposes some informations about CERN network restrictions as part of the Neutron API. It currently supports the command *host-restrictions-show* which, given a hostname, returns the subnets on which the compute service can request a port. The subnets are returned based on different algorithms about the availability of IP addresses.

Both HostRestrictions and SubnetCluster API extensions are implemented in the CERN Service Plugin.

### Allocation pools
In each IP service there are some IP addresses that are reserved an not usable by the cloud infrastructure.
In order to avoid VM creation failures when these addresses are chosen, it is possible to filter these bad addresses out of the availability pool of Neutron subnets.
At the time of creation of a Neutron subnet, the standard API allows to specify which addresses are actually available for allocation using a list of start/end ranges.

## MAC addresses
As within the CERN network there can not be a pair of equal MAC addresses, the cloud infrastructure needs to make sure all ports that are created by Neutron have a MAC address that is reserved to the cloud service.
Neutron allows by default to specify a base MAC address in its configuration file. It will generate a random MAC address based on the base's three most significan bytes.
