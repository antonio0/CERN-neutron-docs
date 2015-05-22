# Instance Metadata

### Problem
Since OpenStack deployments using Neutron generally have instances with overlapping IPs, the Neutron metadata agent exists to act as a proxy between the instances and the Nova metadata service in order to determine from which instance the metadata request is actually coming from.

Neutron metadata agent relies on the tenant network gateway's namespaces in order to function.
When isolated or provider networks are used, this is handled by the DHCP agent's namespace.

Details about how this works can be found at the following links (quite old but still accurate):
- [Metadata via the Quantum router](http://techbackground.blogspot.ie/2013/06/metadata-via-quantum-router.html)
- [Metadata via the DHCP namespace](http://techbackground.blogspot.ch/2013/06/metadata-via-dhcp-namespace.html)

Our chosen networking model uses provider networks and an external managed DHCP service, therefore using the Neutron metadata is not possible.

### Solution
The goal here is to have the instances able to communicate with Nova metadata server through the "magic address" (169.254.169.254).
On Linux compute nodes, the LinuxBridge agent has been patched in order to maintain a DNAT rule that translates the magic address to the metadata server address (specified in the configuration files).
On Hyper-V nodes, the KVP service is used to inject the metadata server address directy to the instances.
These solution are analogous to what the cloud infrastrucuture is currently using in production with nova-network.
