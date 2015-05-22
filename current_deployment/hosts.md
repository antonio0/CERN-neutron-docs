# Hosts

## Compute

Tests were carried out using the following 6 physical machines located across different clusters.

| Name | Hypervisor | VM cluster |
| -- | -- | -- |
| p01001534123141 | Hyper-V | NeutronPool S513-C-IP184 |
| p01001534380289 | KVM | NeutronPool S513-C-IP184 |
| p01001534236512 | KVM | NeutronPool S513-C-IP185 |
| p01001534282606 | KVM | NeutronPool S513-C-IP185 |
| p01001534416832 | KVM | NeutronPool S513-C-IP186 |
| p01001534765058 | Hyper-V | NeutronPool S513-C-IP186 |

## Controller

Any other OpenStack service for this deployment is running on the production CERN cloud infrastructure.

| Name | Services |
| -- | -- |
| neutrondev | Neutron (server), Nova (except nova-compute), Glance |
| keystonesdn | Keystone |
| horizonsdn | Horizon |


