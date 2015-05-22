# Architecture
In order to fully understand the integration details on this documentation, it is essential to have a deep understanding of the Neutron codebase and its modular design.

The resources provided by the Neutron API are classified in the codebase against the following model:

| Core API | Extended API |
| -- | -- |
| Networks | Floating IPs |
| Subnets  | FWaaS |
| Ports    | [everything else] |

Everything that is not a Core resource is considered to be an Extension and should be implemented by leveraging the extension-aware design of Neutron.

The following presentation from the OpenStack Hong Kong 2013 Summit provides a quick overview of most concepts illustrated in this chapter: [Video](https://www.openstack.org/summit/openstack-summit-hong-kong-2013/session-videos/presentation/how-to-write-a-neutron-plugin-if-you-really-need-to) and [Slides](http://www.slideshare.net/salv_orlando/how-to-write-a-neutron-plugin-if-you-really-need-to)
