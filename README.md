<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [ansible-kvm](#ansible-kvm)
  - [Build Status](#build-status)
  - [Requirements](#requirements)
  - [Role Variables](#role-variables)
  - [Dependencies](#dependencies)
  - [Example Playbook](#example-playbook)
  - [License](#license)
  - [Author Information](#author-information)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ansible-kvm

An [Ansible](https://www.ansible.com) role to install [KVM](https://www.linux-kvm.org/page/Main_Page)

## Build Status

[![Build Status](https://travis-ci.org/mrlesmithjr/ansible-kvm.svg?branch=master)](https://travis-ci.org/mrlesmithjr/ansible-kvm)

## Requirements

Install [required](./requirements.yml) [Ansible](https://www.ansible.com) roles:

```bash
sudo ansible-galaxy install -r requirements.yml
```

## Role Variables

```yaml
---
# defaults file for ansible-kvm

# Defines if ssh should be configured to allow root logins
# mainly for managing KVM/Libvirt remotely using virt-manager or other.
kvm_allow_root_ssh: false

# This setting allows usage of the auditing subsystem to be altered:
#
#   audit_level == 0  -> disable all auditing
#   audit_level == 1  -> enable auditing, only if enabled on host (default)
#   audit_level == 2  -> enable auditing, and exit if disabled on host
kvm_audit_level: '1'

# If set to 1, then audit messages will also be sent
# via libvirt logging infrastructure. Defaults to 0
kvm_audit_logging: '0'

# Set an authentication scheme for UNIX read-only sockets
# By default socket permissions allow anyone to connect
kvm_auth_unix_ro: 'none'

# Set an authentication scheme for UNIX read-write sockets
# By default socket permissions only allow root.
kvm_auth_unix_rw: 'none'

# Defines if kvm/libvirt should be configured
kvm_config: false

# Defines if kvm_users should be added to libvirtd for managing KVM
kvm_config_users: false

# Defines if kvm virtual networks should be configured
# if set to true ensure that your underlying bridges have been created
# using mrlesmithjr.config-interfaces role from Ansible Galaxy.
kvm_config_virtual_networks: false

kvm_debian_packages:
  - 'bridge-utils'
  - 'libvirt-bin'
  - 'lldpd'
  - 'python-libvirt'
  - 'python-lxml'
  - 'qemu-kvm'

# Disable apparmor
kvm_disable_apparmor: false

# Flag toggling mDNS advertizement of the libvirt service.
kvm_enable_mdns: false

kvm_enable_system_tweaks: false

# Listen for unencrypted TCP connections on the public TCP/IP port
kvm_enable_tcp: false

# Defines if remote tls connections are desired
# default is true.
kvm_enable_tls: true

kvm_enable_libvirtd_syslog: false

# Define default disk image cache mode
## none, writethrough, writeback, directsync, and unsafe
kvm_images_cache_mode: 'none'

kvm_images_format_type: 'qcow2'

# Defines the path to store VM images
kvm_images_path: '/var/lib/libvirt/images'

# This allows libvirtd to detect broken client connections or even
# dead clients.  A keepalive message is sent to a client after
# keepalive_interval seconds of inactivity to check if the client is
# still responding; keepalive_count is a maximum number of keepalive
# messages that are allowed to be sent to the client without getting
# any response before the connection is considered broken.
kvm_keepalive_interval: '5'
kvm_keepalive_count: '5'
kvm_admin_keepalive_interval: '5'
kvm_admin_keepalive_count: '5'

# Override the default configuration which binds to all network
# interfaces. This can be a numeric IPv4/6 address, or hostname
kvm_listen_addr: '0.0.0.0'

# Logging level: 4 errors, 3 warnings, 2 information, 1 debug
kvm_log_level: '3'

# Defines if VMs defined in kvm_vms are managed
kvm_manage_vms: false

# The maximum length of queue of accepted but not yet
# authenticated clients. The default value is 20. Set this to
# zero to turn this feature off.
kvm_max_anonymous_clients: '20'

# Limit on concurrent requests from a single client
# connection. To avoid one client monopolizing the server
# this should be a small fraction of the global max_requests
# and max_workers parameter
kvm_max_client_requests: '5'
kvm_admin_max_client_requests: '5'

# The maximum number of concurrent client connections to allow
# over all sockets combined.
kvm_max_clients: '5000'
kvm_admin_max_clients: '5'

# The maximum length of queue of connections waiting to be
# accepted by the daemon.
kvm_max_queued_clients: '1000'
kvm_admin_max_queued_clients: '5'

# Total global limit on concurrent RPC calls. Should be
# at least as large as max_workers. Beyond this, RPC requests
# will be read into memory and queued. This directly impacts
# memory usage, currently each request requires 256 KB of
# memory. So by default up to 5 MB of memory is used
#
# XXX this isn't actually enforced yet, only the per-client
# limit is used so far
kvm_max_requests: '20'

# The minimum limit sets the number of workers to start up
# initially. If the number of active clients exceeds this,
# then more threads are spawned, up to max_workers limit.
# Typically you'd want max_workers to equal maximum number
# of clients allowed
kvm_max_workers: '20'
kvm_min_workers: '5'
kvm_admin_min_workers: '1'
kvm_admin_max_workers: '5'

# This allows to specify a timeout for openvswitch calls made by
# libvirt. The ovs-vsctl utility is used for the configuration and
# its timeout option is set by default to 5 seconds to avoid
# potential infinite waits blocking libvirt.
kvm_ovs_timeout: '5'

# The number of priority workers. If all workers from above
# pool are stuck, some calls marked as high priority
# (notably domainDestroy) can be executed in this pool.
kvm_prio_workers: '5'

kvm_redhat_packages:
  - 'bridge-utils'
  - 'libvirt-client'
  - 'libvirt-python'
  - 'libvirt'
  - 'qemu-img'
  - 'qemu-kvm'
  - 'virt-install'
  - 'virt-manager'
  - 'virt-viewer'

# I experienced an issue with bridges no longer working on Ubuntu 16.04
# for some reason. And the following settings below from the link provided
# solved this issue.
# https://wiki.libvirt.org/page/Networking#Debian.2FUbuntu_Bridging
kvm_sysctl_settings:
  - name: 'net.bridge.bridge-nf-call-ip6tables'
    value: 0
    state: "present"
  - name: 'net.bridge.bridge-nf-call-iptables'
    value: 0
    state: "present"
  - name: 'net.bridge.bridge-nf-call-arptables'
    value: 0
    state: "present"

# Override the port for accepting insecure TCP connections
# This can be a port number, or service name
kvm_tcp_port: '16509'

# Override the port for accepting secure TLS connections
# This can be a port number, or service name
kvm_tls_port: '16514'

# Set the name of the directory in which sockets will be found/created.
kvm_unix_sock_dir: '/var/run/libvirt'

# Defines users to add to libvirt group
kvm_users:
  - 'remote'

# Define KVM Networks to create
kvm_virtual_networks: []
  # - name: 'DMZ_ORANGE_VLAN100'
  #   mode: 'bridge'
  #   bridge_name: 'vmbr100'
  #   autostart: true
  #   # active, inactive, present and absent
  #   state: active
  # - name: 'Green_Servers_VLAN101'
  #   mode: 'bridge'
  #   bridge_name: 'vmbr101'
  #   autostart: true
  #   state: active

# Define VM(s) to create
kvm_vms: []
  # - name: 'test_vm'
  #   # Define boot devices in order of preference
  #   boot_devices:
  #     - 'network'
  #     - 'hd'
  #     # - 'cdrom'
  #   graphics: false
  #   # Define disks in MB
  #   disks:
  #       # ide, scsi, virtio, xen, usb, sata or sd
  #     - disk_driver: 'virtio'
  #       name: 'test_vm.1'
  #       size: '36864'
  #     - disk_driver: 'virtio'
  #       name: 'test_vm.2'
  #       size: '51200'
  #   # Define a specific host where the VM should reside..Match inventory_hostname
  #   # host: 'kvm01'
  #   # Define memory in MB
  #   memory: '512'
  #   network_interfaces:
  #     - source: 'default'
  #       network_driver: 'virtio'
  #       type: 'network'
  #     # - source: 'vmbr102'
  #     #   network_driver: 'virtio'
  #     #   type: 'bridge'
  #   state: 'running'
  #   vcpu: '1'
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: kvm_hosts
  become: true
  vars:
  roles:
    - role: ansible-kvm
  tasks:
```

## License

MIT

## Author Information

Larry Smith Jr.

-   [@mrlesmithjr](https://www.twitter.com/mrlesmithjr)
-   [EverythingShouldBeVirtual](http://everythingshouldbevirtual.com)
-   [mrlesmithjr.com](http://mrlesmithjr.com)
-   mrlesmithjr [at] gmail.com
