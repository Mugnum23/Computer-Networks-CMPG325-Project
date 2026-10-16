# Client Requirements Document
## Tau Metalworks & Engineering Supplies

| Field | Detail |
|---|---|
| Project ID | CMPG325-2026-034 |
| Client ID | CLI-034 |
| Module | CMPG325 – Computer Networks |
| Institution | North-West University (NWU) |
| Student | T. Magwatane (42710677) |
| Simulation platform | Cisco Packet Tracer |
| Assigned address block | 172.30.16.0/23 |
| Assigned networking challenge | HTTP/Web Server (internal web service hosting) |
| Design constraint | Critical services must remain available during business hours |
| Change request | CR2 – additional floor |
| Project status | Implemented and verified in Cisco Packet Tracer (final submission, 16 October 2026) |

---

## 1. Client Background

Tau Metalworks & Engineering Supplies is a manufacturing organisation based in Potchefstroom. The organisation requires a reliable, logically segmented network that supports its departments, internal communication and an internal web service, and that can be extended to an additional floor.

The network is simulated in Cisco Packet Tracer to demonstrate IP addressing, VLAN segmentation, inter-VLAN routing, DHCP, server connectivity, trunk links and link aggregation.

## 2. Problem Statement

The organisation requires a structured network that connects its departmental users and server infrastructure while separating departmental traffic into appropriate logical networks. The network must accommodate an additional floor without redesign of the existing IP addressing plan, provide automatic IP configuration for client devices, and make the internal HTTP web server accessible to users across the network.

## 3. Project Objectives

1. Design a network using the allocated IPv4 block `172.30.16.0/23`.
2. Apply Variable Length Subnet Masking (VLSM).
3. Separate departments and services using VLANs.
4. Configure inter-VLAN routing on a multilayer core switch.
5. Provide DHCP address allocation for departmental and new-floor clients.
6. Configure a server with a static IP address and enable HTTP.
7. Connect access switches to the core through 802.1Q trunk links.
8. Implement an LACP EtherChannel between the core switch and the server access switch.
9. Integrate the additional floor using VLAN 70.
10. Test addressing, routing, connectivity and access to the HTTP service.

## 4. Functional Requirements

| ID | Requirement | Implementation / Verification |
|---|---|---|
| FR-01 | The network must use the allocated `172.30.16.0/23` block. | VLSM plan; see `03-ip-addressing/ip-addressing-plan.md`. |
| FR-02 | Departmental and service traffic must be logically separated. | VLANs 10, 20, 30, 40, 50, 60, 70 and 99. |
| FR-03 | User devices must receive IP configuration automatically. | DHCP pools on the Core for VLANs 10, 20, 30, 40, 50 and 70. |
| FR-04 | The HTTP server must use a static IP address. | `172.30.17.66/27`, gateway `172.30.17.65`. |
| FR-05 | The core switch must route traffic between VLANs. | `ip routing` and one SVI per VLAN. |
| FR-06 | Access switches must connect to the core using trunk links. | 802.1Q trunks with VLAN 99 as native VLAN. |
| FR-07 | The server switch must have an aggregated uplink to the core. | LACP EtherChannel `Po1`: Core Fa0/6–Fa0/7, SW-SERVER Fa0/23–Fa0/24. |
| FR-08 | The additional floor must be integrated into the network. | VLAN 70, `172.30.17.96/27`, gateway `172.30.17.97`. |
| FR-09 | Infrastructure must have a dedicated management VLAN. | VLAN 99, `172.30.17.224/28`, Core `172.30.17.225`. |
| FR-10 | The internal HTTP service must be accessible across the network. | Browser access to `http://172.30.17.66` from VLANs 10, 30 and 70. |

## 5. VLAN and IP Addressing Requirements

| VLAN | Department / Purpose | Network | Subnet Mask | Default Gateway |
|---:|---|---|---|---|
| 10 | Administration | `172.30.16.0/26` | `255.255.255.192` | `172.30.16.1` |
| 20 | Sales | `172.30.16.64/26` | `255.255.255.192` | `172.30.16.65` |
| 30 | Engineering | `172.30.16.128/26` | `255.255.255.192` | `172.30.16.129` |
| 40 | Production | `172.30.16.192/26` | `255.255.255.192` | `172.30.16.193` |
| 50 | Warehouse | `172.30.17.0/26` | `255.255.255.192` | `172.30.17.1` |
| 60 | Servers | `172.30.17.64/27` | `255.255.255.224` | `172.30.17.65` |
| 70 | New Floor (CR2) | `172.30.17.96/27` | `255.255.255.224` | `172.30.17.97` |
| 99 | Network Management | `172.30.17.224/28` | `255.255.255.240` | `172.30.17.225` |

The range `172.30.17.128` – `172.30.17.223` is unallocated and reserved for growth.

## 6. Server Requirements

The network must include an internal HTTP server in the server VLAN, with the following settings:

- **IP address:** `172.30.17.66`
- **Subnet mask:** `255.255.255.224`
- **Default gateway:** `172.30.17.65`
- **VLAN:** 60
- **Service:** HTTP

The server must be reachable from client VLANs through the core switch, and access must be verified with documented test evidence.

## 7. Availability Requirement and Link Design

**Requirement:** the client requires that critical services remain available during business hours.

**Response:** the design uses a hierarchical extended-star topology. The server is isolated in VLAN 60 on its own /27 subnet. An LACP EtherChannel combines two Fast Ethernet links between the core and the server switch, providing link-level resilience if one member link fails, provided the remaining link continues operating correctly.

**Limitation:** the design retains a single core switch. Full core redundancy is not provided, and the core remains a potential single point of failure. A link-failure test was not performed.

## 8. Additional Floor Requirements (CR2)

The additional floor must be integrated using VLAN 70. The subnet `172.30.17.96/27` is allocated to it with gateway `172.30.17.97`. Client devices receive addresses through DHCP and communicate with permitted network services without any change to the existing departmental subnets. The floor is served by access switch SW-NEWFLOOR, connected to the core by a standard trunk allowing VLANs 70 and 99.

## 9. Security and Network Management

Logical separation is provided by VLAN segmentation and a dedicated management VLAN (VLAN 99, also the native VLAN on all trunks). Each trunk permits only the VLANs required on that link.

VLAN segmentation does not replace firewall policies, access control lists or comprehensive administrative access controls. Port security, access control lists and secure remote management (SSH) were not implemented and are identified as future work.

## 10. Acceptance and Testing Criteria

- All required VLANs exist and are active.
- VLAN interfaces have the expected gateway addresses and are up.
- DHCP clients receive addresses from the correct subnet.
- Inter-VLAN connectivity works where intended.
- Trunk links carry the required VLANs.
- The LACP EtherChannel is operational and its member ports are bundled.
- The HTTP server uses the specified static address and is reachable from representative client VLANs.
- CR2 clients receive valid DHCP settings and can reach the HTTP service.
- Management addresses of all switches respond.

Results must be supported by Packet Tracer screenshots, command output or browser evidence, and a requirement is marked as passed only after its test succeeds. See `05-testing/testing-log.md`.

## 11. Scope and Limitations

**In scope:** IPv4 subnetting and VLSM; VLAN creation and segmentation; DHCP; inter-VLAN routing; 802.1Q trunks; internal HTTP server; LACP EtherChannel; CR2 integration; management VLAN; connectivity testing in Cisco Packet Tracer.

**Not included:** redundant core switches; DNS services; access control lists and firewall policies; port security; SSH and administrative authentication; production hardware deployment; failover and performance testing.

## 12. Expected Outcome

A functional simulated network that supports the departmental VLANs, automatic client addressing, inter-VLAN communication, an internal HTTP service and the additional floor. The Packet Tracer project and its test evidence demonstrate how the design meets the documented requirements and identify limitations that a production deployment would need to address.
