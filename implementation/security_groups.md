# Security Groups

### Problem
Neutron defines the default security group for every tenant as following:
- For the default security group a rule which allows intercommunication among hosts associated with the default security group is defined by default.
- As a result, all egress traffic and intercommunication in the default group are allowed and all ingress from outside of the default group is dropped by default (in the default security group).

When using a flat provider networking model, these default rules do not allow an instance to communicate with the external network and this can be confusing to the users, since the only way to access their VM would be to change the rules manually for each of their tenant.

### Solution
A patch has been introduced within the default security group's creation code that generates the rules by allowing all the traffic both ingress/egress. In this way, users do not need to be aware of the security group functionality but they can use it if they need.

This issue is potentially addressable upstream as it seems that this is a fairly common problem and nova-network supports the functionality of allowing the administrator to set default rules via the API since Grizzy ([blueprint](https://blueprints.launchpad.net/nova/+spec/default-rules-for-default-security-group)).
An analogous [functionality](https://blueprints.launchpad.net/neutron/+spec/default-rules-for-default-security-group) has been proposed in Neutron for the Kilo release, the implementation started but was later abandoned.
