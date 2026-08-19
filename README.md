# Redundant Multi-Floor Enterprise Campus Network

A redundant, segmented and security-focused enterprise campus network designed and implemented in **Cisco Packet Tracer**.

## Project Overview

This project simulates a multi-floor enterprise campus network with redundant campus routers, redundant Layer-3 distribution switches and three access switches. Departmental traffic is segmented with VLANs, default gateways use HSRP, dynamic routing uses OSPF, and the distribution switches use LACP EtherChannel for resilient Layer-2 connectivity.

Security and management features include SSH version 2, local authentication, a dedicated Management VLAN, ACL-based traffic filtering, DHCP snooping, Dynamic ARP Inspection, Port Security, PortFast and BPDU Guard.

## Architecture

- **Campus routers:** R-CAMP1, R-CAMP2
- **Distribution:** DSW1, DSW2
- **Access:** ASW-F1, ASW-F2, ASW-F3
- **Management VLAN:** VLAN 99
- **Native/unused VLAN:** VLAN 999
- **Platform:** Cisco Packet Tracer

The topology uses redundant uplinks from the floor access switches to both distribution switches.

## VLAN & IP Addressing

| VLAN | Department / Function | Network | HSRP Virtual Gateway |
|---:|---|---|---|
| 10 | ADMIN | 10.10.10.0/24 | 10.10.10.1 |
| 20 | HR | 10.10.20.0/24 | 10.10.20.1 |
| 30 | FINANCE | 10.10.30.0/24 | 10.10.30.1 |
| 40 | DEVELOPMENT | 10.10.40.0/24 | 10.10.40.1 |
| 50 | TRAINING | 10.10.50.0/24 | 10.10.50.1 |
| 60 | SERVER | 10.10.60.0/24 | 10.10.60.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

DSW1 uses `.2` and DSW2 uses `.3` for the VLAN gateway interfaces. DSW1 is configured as the preferred HSRP gateway with priority 110 and preemption.

## Core Networking Technologies

### VLANs & Trunking
Departmental VLANs provide separate broadcast domains and traffic segmentation. Trunks carry the required VLANs between network layers, with VLAN 999 used as the native/unused VLAN.

### HSRP
HSRP provides a shared virtual default gateway for each VLAN. If the active gateway becomes unavailable, the standby device can take over without changing the end-device gateway.

### OSPF
OSPF process 1 provides dynamic routing across the routed /30 links between the campus routers and distribution layer.

### EtherChannel / LACP
DSW1 and DSW2 use LACP to bundle two physical links into Port-channel 1, providing logical link aggregation and resilience.

### Rapid-PVST
Rapid-PVST provides Layer-2 loop prevention and controlled root placement. DSW1 and DSW2 use primary/secondary root roles across VLAN groups.

### DHCP
DHCP pools provide automatic addressing for the required user VLANs, including default gateway and DNS settings.

### SSH
SSH version 2 with local authentication provides secure remote device management. VTY access is restricted through the management ACL.

### ACLs
ACLs are used to:
- Restrict unauthorized access to the Management VLAN.
- Restrict TRAINING-to-SERVER traffic.
- Control inter-VLAN traffic according to the configured policy.

### Access-Layer Security
The project also includes Port Security, sticky MAC learning, PortFast and BPDU Guard, along with DHCP Snooping and Dynamic ARP Inspection.

## Verification

The project documentation records verification of:

- `show ip interface brief`
- `show vlan brief`
- `show interfaces trunk`
- `show etherchannel summary`
- `show standby brief`
- `show ip ospf neighbor`
- `show ip route`
- `show ip ssh`
- `show access-lists`
- `show running-config | include access-group`
- `show ip dhcp binding`
- End-to-end ping and traceroute tests

The supplied project presentation reports successful verification of VLANs, trunks, STP/PVST+, EtherChannel/LACP, HSRP, inter-VLAN routing, OSPF, DHCP, SSH, ACLs, Port Security, PortFast, BPDU Guard and end-to-end connectivity.

## Troubleshooting Experience

Several real configuration issues were documented and resolved during implementation:

1. **Trunk configuration rejected** — 802.1Q encapsulation had to be explicitly configured before trunk mode.
2. **R-CAMP2 connectivity issue** — incorrect or missing /30 addressing was corrected.
3. **Duplicate transit IP** — an incorrect DSW1 transit address was corrected.
4. **OSPF adjacency failure** — interface status, addressing and OSPF configuration were checked and corrected.
5. **ACL attachment verification** — ACL counters were used as additional functional evidence when Packet Tracer displayed inconsistent interface-level output.
6. **EtherChannel trunk display issue** — Port-channel trunk encapsulation and trunk mode were explicitly configured.

## Repository Contents

```text
Packet-Tracer/
└── redundant-multi-floor-enterprise-campus-network.pkt

Configurations/
└── device-configurations-SANITIZED.txt

Documentation/
└── Project-Presentation.pptx

README.md
LICENSE
.gitignore
```

## Team

- **Vaibhav Mule** — Team Member
- **Dhruv Gaidhane** — Team Member
- **Tejas Bargal** — Team Member

**Organization:** NextGen Education

## Security Note

Credentials and secrets are intentionally removed from the published configuration file. Do not publish real passwords, private keys or other sensitive credentials in a public GitHub repository.

## Future Scope

Possible extensions identified in the project include SNMP monitoring, centralized Syslog, network automation, IPv6, wireless integration, advanced ACL policies, firewall/IDS integration and deployment on physical network equipment.
