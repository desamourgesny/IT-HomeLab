# Linux Network Troubleshooting Lab

## Lab Environment

This lab uses two Linux virtual machines in VirtualBox to practice systematic network troubleshooting.

- Ubuntu Server
- Kali Linux
- NAT adapter for Internet connectivity
- Host-Only adapter for communication between the virtual machines

### Network Configuration

Ubuntu:
- NAT: 10.0.2.15/24

- Host-Only: 192.168.56.101/24

Kali:
- NAT: 10.0.2.15/24
- Host-Only: 192.168.56.102/24

Default Gateway: 10.0.2.2
DNS Server: 10.0.2.3

## Incident 1 - Host-Only Connectivity Failure

### Symptoms

Ubuntu could not reach Kali over the Host-Only network.

- Ping to `192.168.56.50` failed with 100% packet loss.
- `ip neigh` showed the neighbor entry as `FAILED`.
- Kali's NAT interface remained operational.

### Investigation

I checked the Kali network interfaces with:

```bash
ip addr

```markdown
The NAT interface was UP, but the Host-Only interface `eth1` was DOWN.

### Root Cause

The Kali Host-Only interface `eth1` was down.

### Fix

I brought the Kali Host-Only interface back up:

```bash
sudo ip link set eth1 up
### Verification

I tested connectivity from Ubuntu to Kali:

```bash
ping -c 3 192.168.56.50
ip neigh show 192.168.56.50
## Incident 2 - Temporary IP Configuration Lost After Reboot

### Symptoms

Kali had previously been using `192.168.56.50/24` on the Host-Only interface. After a reboot, that address disappeared and `192.168.56.102/24` returned.

### Investigation

I checked the interface configuration with:

```bash
ip addr



### Investigation

I checked the interface configuration with:

```bash
ip addr
After checking the interface with `ip addr`, I found that `eth1` was UP, `192.168.56.102/24` had returned, and the temporary `192.168.56.50/24` address had disappeared.

### Root Cause

The `192.168.56.50/24` address disappeared after Kali rebooted because it was configured temporarily at runtime and was not saved as a persistent network configuration.

### Verification

I tested connectivity from Ubuntu to Kali:

```bash
ping -c 3 192.168.56.102
ip neigh show 192.168.56.102

## Incident3 : DNS Resolution Failure.

### Symptoms
The IP address `8.8.8.8` was reachable, but the hostname `google.com` could not be resolved.
### Investigation

I checked the DNS configuration with:

```bash
resolvectl status

### Root Cause

The DNS server had been manually changed from `10.0.2.3` to `192.0.2.123` to simulate a DNS configuration failure.

### Fix

I restored the correct DNS server on the NAT interface:

```bash
sudo resolvectl dns enp0s3 10.0.2.3
### Verification

I verified the DNS configuration with:

```bash
resolvectl status
