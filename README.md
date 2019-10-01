Role Name
=========

Setup a RHEL7.x server as hypervisor using Libvirt able to PXE provision RHEL server.

Requirements
------------

Multiple packages are needed, mainly:

- libvirt
- bind9
- httpd
- tftp
- dhcp

Role Variables
--------------

main variables:

```yaml
managed_domains:
  - dns_domain: example.com
    network: 192.168.124.0
    netmask: 255.255.255.0
    gateway: 192.168.124.1
    dns: 192.168.124.1
    next_server: 192.168.124.1
    dhcp_range:
        start: 192.168.124.200
        end: 192.168.124.209
    dhcp_boot_range:
        start: 192.168.124.210
        end: 192.168.124.219
  - dns_domain: iaas.example.com
    network: 192.168.125.0
    netmask: 255.255.255.0
    gateway: 192.168.125.1
    dns: 192.168.125.1
    next_server: 192.168.125.1
    dhcp_range:
        start: 192.168.125.200
        end: 192.168.125.209
    dhcp_boot_range:
        start: 192.168.125.210
        end: 192.168.125.219

iso_images:
  - cd: "/srv/rhel-server-7.6-x86_64-dvd.iso"
    path: "/var/www/html/rhel/7.6/server/x86_64/os"
    desc: "RHEL-7.6"
  - cd: "/srv/rhel-server-7.4-x86_64-dvd.iso"
    path: "/var/www/html/rhel/7.4/server/x86_64/os"
    desc: "RHEL-7.4"
  - cd: "/srv/rhel-server-7.3-x86_64-dvd.iso"
    path: "/var/www/html/rhel/7.3/server/x86_64/os"
    desc: "RHEL-7.3"


boot_message: "BCGAudio PXE Provisioning Server"
install_packages: true

static_hosts:
    - hostname: "bananapi"
      macaddress: "02:4b:07:00:00:00"
      ipaddress: "192.168.133.90"
```

daemon configuration files and data directories (as tftp, bind of httpd) are left unchanged.

packages installation is tied to this variable, set to false by default, change it to true in order to install packages

```yaml
install_packages: true
```

Dependencies
------------

NA

Example Playbook
----------------

```yaml
- name: setup libvirt environment
  hosts: localhost
  vars_files:
    - environmental-variables/main.yaml
  roles:
    - ansible-LibvirtSetup
```

NOTE: permit communications within multiple bridges interfaces

```bash
firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -o virbr4 -j ACCEPT
firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -i virbr3 -j ACCEPT
firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -i virbr4 -j ACCEPT
firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -o virbr3 -j ACCEPT
firewall-cmd --reload
```

License
-------

BSD

Author Information
------------------

Marco Guarnieri  
mguarnieri@outlook.es  
January '18  
