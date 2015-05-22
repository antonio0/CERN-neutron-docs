# Improvements

# IPv6
Neutron fully supports IPv6 networks. When creating a subnet, the user can specify an *ip_version* that is either *4* or *6*. Any successive request for ports on that subnet would generate an IP address based on the version attribute.
However, CERN IP services assign both an IPv4 and IPv6 address for all network interfaces.
In order to introduce IPv6 functionalities on our Neutron deploy, we need to map all IPv4 ports to IPv6 addresses/pools and vice versa.

A possible way of mitigating this issue involves the following:
- create two subnets (v4 and v6) on the provider network with the same name (IP service name)
- allow users to create either IPv4 instances or IPv6 instances
- if an IPv4 instance is created, create a dummy port (with fixed IPv6 address obtained from LanDB) on the IPv6 subnet right after LanDB registration (this behaviour can be implemented as part of the LanDB mechanism driver). The opposite applies if the user chooses to create an instance on an IPv6 network. The dummy port shall not get registered in LanDB, as the registration would fail.
- when the instance is deleted, take care of deleting the dummy port

This solution would avoid patching the Neutron API, plugin and database schema.
In case having ports with both type of addresses in OpenStack is a strict requirement, a hack on Neutron subnets and ports is inevitable.

# Liberty's IPAM support
One of the main new features offered by Neutron on the OpenStack Liberty release is the possibility of writing pluggable drivers that interact with external IPAM systems ([spec](http://specs.openstack.org/openstack/neutron-specs/specs/liberty/neutron-ipam.html)).

It is worth investigating whether it is possible to benefit from it by turning the HostRestrictions API implementation into a IPAM driver that automatically selects the subnets. If this is possible, it won't be necessary to offer the HostRestrictions API and thus remove the nova call that selects a subnet available for the host.
