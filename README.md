Role Name
=========

A simple role to setup a dhcp service in a server with one or multiple nics, serving directly connected or remote subnets.

Requirements
------------

NA

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
|options|list(string)|dhcp options for the subnet. See https://www.iana.org/assignments/bootp-dhcp-parameters/bootp-dhcp-parameters.xhtml for full list.
|fixed_ip_hosts|list(dict)|Fixed hosts in the subnets

**fixed_ip_hosts attributes**
|fixed_ip_hosts|Type|Description
|---|---|---
|name|string|The name of the fixed host
|hardware_address|string|The MAC address of the host
|fixed_ip_address|string|The ip address of the host


```
dhcpd_package: dhcp-server-12:4.4.2-11.b1.fc34

dhcpd_service_nics: 
  - eth0

dhcpd_subnets:
  - name: test1
    directly_connected_to_interface: yes
    provides_service: yes
    network: a.b.c.d
    netmask: e.f.g.h
    range: a.b.c.d e.f.g.h;
    options:
      - "option value"
    fixed_ip_hosts:
      - name: server1.example.com
        hardware_address: AA:BB:CC:DD:EE
        fixed_ip_address: a.b.c.d
      - name: server2.example.com
        hardware_address: AA:BB:CC:DD:E1
        fixed_ip_address: e.f.g.h
  - name: test2
    directly_connected_to_interface: yes
    provides_service: yes
    network: a.b.c.d
    netmask: e.f.g.h
    range: a.b.c.d e.f.g.h;
    options:
      - "option value"
    fixed_ip_hosts:
      - name: server1.example.com
        hardware_address: AA:BB:CC:DD:EE
        fixed_ip_address: a.b.c.d
      - name: server2.example.com
        hardware_address: AA:BB:CC:DD:E1
        fixed_ip_address: e.f.g.h
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
          - eth0
        dhcpd_subnets:
          - name: test1
            directly_connected_to_interface: yes
            provides_service: yes
            network: a.b.c.d
            netmask: e.f.g.h
            range: a.b.c.d e.f.g.h;
            options:
              - "option value"
            fixed_ip_hosts:
              - name: server1.example.com
                hardware_address: AA:BB:CC:DD:EE
                fixed_ip_address: a.b.c.d
              - name: server2.example.com
                hardware_address: AA:BB:CC:DD:E1
                fixed_ip_address: e.f.g.h
          - name: test2
            directly_connected_to_interface: yes
            provides_service: yes
            network: a.b.c.d
            netmask: e.f.g.h
            range: a.b.c.d e.f.g.h;
            options:
              - "option value"
            fixed_ip_hosts:
              - name: server1.example.com
                hardware_address: AA:BB:CC:DD:EE
                fixed_ip_address: a.b.c.d
              - name: server2.example.com
                hardware_address: AA:BB:CC:DD:E1
                fixed_ip_address: e.f.g.h
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