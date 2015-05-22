# Configuration

Puppet has been used to configure the entire test cluster.
CERN shared puppet modules have been used to deploy all OpenStack services except Neutron.

The Neutron [puppet module](https://github.com/stackforge/puppet-neutron) from [StackForge](http://docs.openstack.org/infra/system-config/stackforge.html) has been imported and modified in order to add custom configuration parameters.
However, this module is only compatible with the major Linux distributions. Therefore, in order to automate the deployment of the Neutron Hyper-V agents on Windows compute nodes, a new [module](https://github.com/antonio0/puppet-neutron_hyper_v) has been written from scratch and imported to the CERN repository.
