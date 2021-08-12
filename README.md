infra_server_dhcp_dhcpd
=========

A simple role to setup a dhcp service in a server with one or multiple nics, serving both direct and remote subnets.

Requirements
------------

* The dhcpd configuration provided MUST be correct. The role DOES NOT validate if the subnet values are correct or if something is missing at this version.
* You also need to make sure that there is at least one subnet that is listening regardless if it serves a local subnet or not.

Releases
------------

|Branch|Status|Description| Date
|---	|---	|---	|---
|master  	| Unstable	| Development Branch | Now
|1.0.0  	| Release  	| Release 1.0.0 | 12-08-2021



Role Variables
--------------

**top level vars**
|Variable|Level|Description
|---|---|---	
|dhcpd_service_nics|Default|A list of network interfaces that listen for dhcp requests
|dhcpd_subnets|Default|A list of subnets which will be served by the dhcp service
|dhcpd_package|Default|The dhcpd package in a valid NEVRA or NSVCA form. https://docs.fedoraproject.org/en-US/modularity/architecture/nsvca/  	

**dhcpd_subnets attributes**
|dhcpd_subnets|Type|Description
|---|---|---
|name|string|The name of the subnet
|directly_connected_to_interface|boolean|Whether the subnet is connected to a physical nic on the server
|provides_service|boolean|Whether the subnet is also served by the dhcp service.
|network|string|The network address of the subnet
|netmask|string|The netmask address of the subnet
|range|string|The ip range of the subnet served by the dhcp service
|options|list(string)|dhcp options for the subnet. You need to include the 'option' keyword if the option requires it. See https://www.iana.org/assignments/bootp-dhcp-parameters/bootp-dhcp-parameters.xhtml for full dhcp options list.
|fixed_ip_hosts|list(dict)|Fixed hosts in the subnets

**fixed_ip_hosts attributes**
|fixed_ip_hosts|Type|Description
|---|---|---
|name|string|The name of the fixed host
|hardware_address|string|The MAC address of the host
|fixed_ip_address|string|The ip address of the host


```
dhcpd_package: dhcp-server-12:4.3.6-44.0.1.el8

dhcpd_service_nics: 
  - tap0

dhcpd_subnets:
  - name: direct_net
    directly_connected_to_interface: yes
    provides_service: yes
    network: 10.0.2.0
    netmask: 255.255.255.0
    range: 10.0.2.1 10.0.2.100
    options:
      - "option domain-name-servers 192.0.2.1"
    fixed_ip_hosts:
      - name: server1.direct.local
        hardware_address: B7:55:40:09:AD:17
        fixed_ip_address: 10.0.2.102
      - name: server2.direct.local
        hardware_address: E0:14:50:B2:2A:E1
        fixed_ip_address: 10.0.2.105
  - name: external_net
    directly_connected_to_interface: no
    provides_service: yes
    network: 10.0.3.0
    netmask: 255.255.255.0
    range: 10.0.3.1 10.0.3.110
    options:
      - "option domain-name-servers 192.0.2.2"
    fixed_ip_hosts:
      - name: server1.remote.external
        hardware_address: CE:F9:41:68:CB:E8
        fixed_ip_address: 10.0.3.121
      - name: server2.remote.external
        hardware_address: F4:E6:04:5E:6C:80
        fixed_ip_address: 10.0.3.124
```





Dependencies
------------
NA


Example Playbook
----------------


```
---
- hosts: dhcp_server
  become: yes
  roles:
    - role: infra_server_dhcp_dhcpd
      vars:
        dhcpd_service_nics: 
          - tap0
        dhcpd_subnets:
          - name: direct_net
            directly_connected_to_interface: yes
            provides_service: yes
            network: 10.0.2.0
            netmask: 255.255.255.0
            range: 10.0.2.1 10.0.2.100
            options:
              - "option domain-name-servers 192.0.2.1"
            fixed_ip_hosts:
              - name: server1.direct.local
                hardware_address: B7:55:40:09:AD:17
                fixed_ip_address: 10.0.2.102
              - name: server2.direct.local
                hardware_address: E0:14:50:B2:2A:E1
                fixed_ip_address: 10.0.2.105
          - name: external_net
            directly_connected_to_interface: no
            provides_service: yes
            network: 10.0.3.0
            netmask: 255.255.255.0
            range: 10.0.3.1 10.0.3.110
            options:
              - "option domain-name-servers 192.0.2.2"
            fixed_ip_hosts:
              - name: server1.remote.external
                hardware_address: CE:F9:41:68:CB:E8
                fixed_ip_address: 10.0.3.121
              - name: server2.remote.external
                hardware_address: F4:E6:04:5E:6C:80
                fixed_ip_address: 10.0.3.124
```

License
-------

Apache-2.0

Author Information
------------------

```
Petros Petrou
email: ppetrou@gmail.com
web: www.petrospetrou.co.uk
```
