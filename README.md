
# Ansible Role:  `dhcpd`


Installs and configure a dhcp server on varoius linux systems.


[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/bodsch/ansible-dhcpd/CI)][ci]
[![GitHub issues](https://img.shields.io/github/issues/bodsch/ansible-dhcpd)][issues]
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/bodsch/ansible-dhcpd)][releases]

[ci]: https://github.com/bodsch/ansible-dhcpd/actions
[issues]: https://github.com/bodsch/ansible-dhcpd/issues?q=is%3Aopen+is%3Aissue
[releases]: https://github.com/bodsch/ansible-dhcpd/releases


## tested operating systems

* Debian 9 / 10
* Ubuntu 18.04 / 20.04
* CentOS 7 / 8
* Oracle Linux 7 / 8
* Arch Linux
* Artix Linux


## usage

```yaml
dhcpd_server_global:
  authoritative: false
  # Default lease time for all IP address leases (18 hours)
  default_lease_time: '{{ (((dhcpd_lease_time | int / 2) + 6) * 60 * 60) | round | int }}'
  # Maximum lease time for all IP addresses (24 hours)
  max_lease_time: '{{ (dhcpd_lease_time | int * 60 * 60) | round | int }}'
  # Log facility to use
  log_facility: 'local7'
  options:
    domain_name: "{{ ansible_domain }}"
    routers: []

dhcpd_ddns:
  update_style: standard

dhcpd_subnets: {}

dhcpd_groups: {}

dhcpd_hosts: {}

dhcpd_interfaces: {}

dhcpd_auto_options: false
```

## examples

```yaml
dhcpd_server_global:
  authoritative: false
  default_lease_time: 600
  max_lease_time: 7200
  options:
    domain_name: "molecule.test"
    domain_search: "molecule.test"
    domain_name_servers:
      - 192.168.1.5
      - 141.1.1.1

dhcpd_interfaces:
  ipv4:
    - eth0
  ipv6: []
```

### `dhcpd_subnets`

```yaml
dhcpd_subnets:
  "LOCAL net":
    comment: 'ipv4 - generated automatically by Ansible'
    subnet: '{{ ansible_default_ipv4.network + "/" + ansible_default_ipv4.netmask }}'
    routers:
      - 192.168.1.1
    options:
      default-lease-time: 2400
      max-lease-time: 7200
```


### `dhcpd_groups`

```yaml
dhcpd_groups:
  infrastructure:
    comment: "infrastructure"
    options:
      domain_name: "molecule.test"
      domain_search: "molecule.test"
      domain_name_servers:
    hosts:
      nas:
        comment: "simply the NAS"
        mac: 00:08:9b:f9:yy:xx
        address: 192.168.1.6
        options:
          host_name: "nas"
          max_lease_time: 1200
```


### `dhcpd_hosts`

```yaml

dhcpd_hosts:
  brennstuhl:
    comment: socket with LAN
    address: 192.168.1.4
    mac: 20:4C:6D:00:yy:xx
```
