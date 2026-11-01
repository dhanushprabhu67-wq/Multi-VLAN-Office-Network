# Multi-VLAN Office Network — Cisco Packet Tracer

A simulated departmental office network using **VLAN segmentation**, **router-on-a-stick inter-VLAN routing**, **router-based DHCP**, and **switch port security**.

## Topology

![Packet Tracer network topology](screenshots/topology.png)

- Router0 `GigabitEthernet0/0` ↔ Switch0 `FastEthernet0/1` (802.1Q trunk)
- Six PCs connected to Switch0 access ports `Fa0/2`–`Fa0/7`
- Three departments, each on a separate VLAN and IPv4 subnet

## VLAN and addressing plan

| VLAN | Department | Network | Router gateway | Switch access ports | DHCP leases observed |
|---|---|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 | Fa0/2–3 | 192.168.10.10, .11 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 | Fa0/4–5 | 192.168.20.10, .11 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | Fa0/6–7 | 192.168.30.10, .11 |

Router subinterfaces `Gi0/0.10`, `Gi0/0.20`, and `Gi0/0.30` use 802.1Q tags and provide the default gateways. DHCP excludes `.1` through `.9` in each subnet.

## Features

- VLANs 10/20/30 with access ports assigned per department
- Switch trunk on `Fa0/1` allowing VLANs 10, 20, 30
- Router-on-a-stick inter-VLAN routing
- DHCP pools for HR, Sales, IT
- Port security on access ports `Fa0/2`–`Fa0/7`: maximum 1 MAC, sticky learning, violation mode shutdown
- PortFast and BPDU Guard configured on PC-facing ports per project configuration steps

## Evidence and verification

| Check | Evidence | Result |
|---|---|---|
| VLAN membership | [VLAN output](screenshots/vlan-brief.png) | Three active VLANs, expected ports |
| Trunk | [Trunk output](screenshots/trunk-status.png) | Fa0/1 trunking, VLANs 10/20/30 forwarding |
| Port-security configuration | [Port-security summary](screenshots/port-security-summary.png) | Six ports, maximum 1 MAC, shutdown action, zero violations at capture |
| Sticky MAC learning | [Secure MAC table](screenshots/sticky-mac.png) | One sticky MAC learned on Fa0/2; remaining ports not yet shown learned |
| DHCP | [DHCP bindings](screenshots/dhcp-bindings.png) | Six automatic leases across three subnets |
| Routing | [Routing table](screenshots/routing-table.png) | All three VLAN subnets directly connected |
| Inter-VLAN connectivity | User-reported successful PC0→PC2 ping | Passed per user report; screenshot not included |
| Port-security violation | User-confirmed lab test; screenshot not included | **Completed (reported by project author)** |

The routing table has no default route, which is expected for this internal-only topology. This project does not implement ACLs or internet access.

## Open the project

1. Install Cisco Packet Tracer.
2. Open [`Multi-VLAN-Office-Network.pkt`](Multi-VLAN-Office-Network.pkt).
3. Inspect Router0, Switch0, and PCs; use the CLI commands in [verification guide](docs/verification.md).

## Configuration reference

The files in [`configs/`](configs/) reconstruct the commands used in the guided build. They are **not device-exported running configurations**. The `.pkt` file is the source for the actual saved simulation.

## Resume-ready description

Built a router-on-a-stick topology with inter-VLAN routing to segment an office network by department/function. Applied port security to restrict unauthorized device access at the switch port level. Documented the design, configuration, and topology diagrams as a networking portfolio project.
