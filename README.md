# Ansible role 'rh-network'

An Ansible role designed to configure basic network interfaces for CentOS 7. This role supports configuration for:

- DHCP
- Static IP
- Default route manipulation
- DNS server
- DNS search domain

## Requirements
- The OS needs to be CentOS 7, OS' in the same family (Fedora, RHEL) might work but are not officially supported.

## Dependencies
This role has no dependencies.

## Role variables

| Variable                      | Required | Default                       		| Comments											  |
| :---                          | :---	   | :---                          		| :---          										  |
| `rhnetwork-scriptsLocation`   | No 	   | '/etc/sysconfig/network-scripts/ifcfg-'	| This is the location where the network scripts are stored + prefix				  |
| `rhnetwork_interfaces`	| Yes	   | -						| List of dicts specifying which network interfaces should be configured. See below for examples. |

## Configuring interfaces
Interface configurations are specified by dicts like this:

```Yaml
rhnetwork_interfaces:
  - name: eth0
    ip: 'dhcp'
    defroute: 'no'

  - name: eth1
    ip: 192.168.0.150
    netmask: 255.255.255.0
    gateway: 192.168.0.1
    defroute: 'yes'
    dnsserver: 192.168.0.1
    dnssearch: 'ansible.lan'
```

| Key		| Required		 | Default	| Comments															|
| :---		| :---			 | :---		| :---																|
| `name`	| Yes			 | -		| The name of the interface to configure											|
| `ip`		| No			 | 'dhcp'	| This can be the static ip address (X.X.X.X) or (default) 'dhcp' to use DHCP							|
| `netmask`	| When static IP is used | -		| The netmask corresponding with the static IP											|
| `gateway`	| No			 | -		| The default gateway corresponding with the connected network									|
| `defroute`	| No			 | 'no'		| Whether or not the default gateway (can be set by DHCP or static) should be added as a default route in the routing table	|
| `dnsserver`	| No			 | -		| The IP address of the DNS server to use											|
| `dnssearch`   | No			 | -		| The DNS search domain to append to hostnames in case no FQDN is specified							|

In the example above, interface 'eth0' is configured with DHCP (IP & DNS) with no default route installed in the routing table.  
Interface 'eth1' is configured with a static IP, Gateway, DNS, DNS Search Domain and it's default route is intalled in the routing table.  

**Remarks:**

(1) It is recommended to set defroute to 'yes' on exactly one interface.
