# LanDB Mechanism Driver

All VMs with a network interface must be registered on the CERN network database (LanDB), which contains the definition of hosts, the associated NICs IP and MAC addresses, the responsible contacts and other fields for systems at CERN.

In order to integrate this registration process within the Neutron port creation/deletion workflow, a dedicated Mechanism Driver has been written.

The generic Mechanism Driver API defines two sets of methods, precommit and postcommit, for every operation (create, update, delete) on a core resource (networks, subnets and ports).
Precommit methods are invoked right before the transaction on the resource's corresponding operation. They should not contain any call outside of Neutron. If an error occurs, the result will be a rollback of the current transaction.
Postcommit methods are invoked right after the transaction has committed. They can contain blocking calls but performance can be affected since the entire process will be blocked. Errors are handled in diverse ways based on the operation that has been performed.

For the purpose of registering and deleting entries on LanDB, two methods of have been implemented, *create_port_postcommit* and *delete_port_postcommit*.
These methods both call LanDB's SOAP interface if the owner of the port is the compute service.
Neutron will trigger a port deletion every time a port creation fails, therefore care should be taken especially when the creation has failed because of the IP address being already registered in LanDB, as that may cause the VM entry for that IP to be deleted.

## Limitations

### Metadata
Neutron does not have a representation of the instance connected to a port. It only has a *device_id* attribute which could refer to a Nova instance ID or any other type of device. The *device_owner* port attribute determines which service is managing the device.
However, LanDB requires more instance-specific attributes that only Nova is aware of. Therefore is it necessary to make a call to the nova service to get all the attributes associated with a specific *device_id*. This is currently done by using the python nova client.

Image metadata need to be retrieved from Glance in order to correctly fill the "OS" and "Version" fields on LanDB.
Querying glance for the image used by the instance connected to the port (we can get *image_id* from the previous call to nova) requires the glance client, which is not a standard requirement for the neutron package.
*This hasn't been implemented yet resulting in all instances being registered with the "Windows" attribute.
*

These issues can be solved by letting Nova take care of the instance registration and Neutron of the interface registration. However, such a technique is not yet supported  by the LanDB API.

Another way to mitigate the issue would involve Neutron registering a device with dummy metadata and Nova update this device with the real metadata after the port has been bound to the instance.

### Wasted IPs
When a call to LanDB as part of *delete_port_postcommit* fails, it means that an IP address would be regarded as free by Neutron but not available by LanDB, which means that all future VM creation on that IP address should fail and the IP is not usable anymore. There should be a system in place that periodically runs consistency checks between LanDB and Neutron to avoid having some wasted IP addresses.
