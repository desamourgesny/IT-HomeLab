\# Network Incident 01: VirtualBox NAT Adapter Failure



\## Scenario



The Ubuntu virtual machine initially had normal network connectivity.



I intentionally disabled Adapter 1 (NAT) in VirtualBox to simulate a network outage and practice a structured troubleshooting process.



\## Baseline



Before creating the failure, I verified network connectivity using:



\- `ip addr`

\- `ip route`

\- `ping 10.0.2.2`

\- `ping 8.8.8.8`

\- `ping google.com`



All baseline tests were successful.



The Ubuntu VM had:



\- NAT interface: `enp0s3`

\- NAT IP address: `10.0.2.15/24`

\- Host-only interface: `enp0s8`

\- Host-only IP address: `192.168.56.101/24`

\- Default gateway: `10.0.2.2`



\## Symptoms After Failure



After disabling Adapter 1 (NAT), the VM lost the network path it normally used to reach its default gateway and the Internet.



The connectivity tests that previously worked were no longer successful.



\## Troubleshooting Process



I followed this troubleshooting order:



1\. `ip addr`

&#x20;  - Checked the network interfaces and IP configuration.



2\. `ip route`

&#x20;  - Checked the routing table and default route.



3\. `ping 10.0.2.2`

&#x20;  - Tested connectivity to the default gateway.



4\. `ping 8.8.8.8`

&#x20;  - Tested Internet connectivity directly by IP without depending on DNS.



5\. `ping google.com`

&#x20;  - Tested hostname resolution and connectivity.



The failure occurred before DNS became the main concern because the VM could not successfully reach its normal gateway/Internet path.



\## Root Cause



VirtualBox Adapter 1, configured as NAT, had been disabled.



This removed the VM's normal NAT path used to reach the default gateway and external networks.



\## Resolution



I re-enabled Adapter 1 (NAT) in VirtualBox.



\## Verification



After restoring the NAT adapter, I repeated the troubleshooting tests:



\- Network interface/IP check: PASS

\- Routing/default gateway check: PASS

\- Default gateway `10.0.2.2`: PASS

\- Public IP `8.8.8.8`: PASS

\- `google.com`: PASS



Network connectivity was successfully restored.



\## What I Learned



I learned that troubleshooting should follow a logical path instead of immediately assuming the cause.



My current troubleshooting flow is:



Interface/IP → Routing → Default Gateway → Internet by IP → DNS/Hostname → Application



I also learned that a failed test helps identify where to investigate next, but one failed test does not always prove the exact root cause.

