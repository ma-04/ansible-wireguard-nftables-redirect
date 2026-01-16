# ansible-wireguard-nftables-redirect

This Ansible project automates the setup of a WireGuard tunnel between two servers (**Origin** and **Destination**) and configures `nftables` on the Origin server to forward specific ports to the Destination server through the tunnel.

## Overview

The playbook performs the following actions:
1.  **Install WireGuard** on both the origin and destination servers.
2.  **Configure WireGuard** interfaces on both servers with a specified IP range.
3.  **Install nftables** on the origin server.
4.  **Connect Traffic**: Configures `nftables` rules on the origin server to forward traffic from specific ports to the destination server.

## Inventory Setup

The inventory must define two host groups: `origin` and `destination`. Each group should contain a single host.

Example `inventory.ini`:

```ini
[origin]
 198.51.100.1 ansible_user=root

[destination]
 203.0.113.1 ansible_user=root
```

## Configuration

Port forwarding and WireGuard settings are controlled via variables.

### Default Variables

By default, the following ports are forwarded: `22`, `80`, `443`, `20022`, `8081`.

### Custom Variables

Create a variable file (e.g., `vars.yml`) to override defaults:

```yaml
# Ports to forward from Origin to Destination
forward_ports:
  - 22
  - 80
  - 443
  - 20022
  - 8081

# WireGuard Configuration
wg_interface: wg0
wg_port: 51820
wg_network_cidr: 10.8.0.0/24
origin_wg_ip: 10.8.0.1
destination_wg_ip: 10.8.0.2
```

## Usage

Run the playbook specifying your inventory and variable file:

```bash
ansible-playbook -i inventory.ini site.yml -e @vars.yml
```