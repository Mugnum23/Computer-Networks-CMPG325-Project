# CMPG325 – Milestone 2

## Tau Metalworks & Engineering Supplies Network Implementation

**Project:** CMPG325 Computer Networks  
**Project ID:** CMPG325-2026-034  
**Client:** Tau Metalworks & Engineering Supplies  
**Milestone:** 2  
**Network:** 172.30.16.0/23

---

## Overview

Milestone 2 implements the network architecture developed during Milestone 1
using Cisco Packet Tracer.

The implementation includes VLAN segmentation, inter-VLAN routing using the
Layer 3 Core switch, centralised DHCP, an internal HTTP server, CR2 network
connectivity, management VLAN configuration and LACP EtherChannel redundancy.

---

## Network Features

- Extended-star / hierarchical network topology
- Layer 3 Core switching
- VLAN segmentation
- Inter-VLAN routing
- Centralised DHCP
- Internal HTTP/Web Server
- VLAN 70 CR2 implementation
- VLAN 99 management network
- LACP EtherChannel
- 802.1Q trunking
- End-to-end connectivity testing

---

## VLANs

| VLAN | Department / Function |
|------|------------------------|
| 10 | Administration |
| 20 | Sales |
| 30 | Engineering |
| 40 | Production |
| 50 | Warehouse |
| 60 | Servers |
| 70 | CR2 / New Floor |
| 99 | Management |

---

## Project Files

### Packet Tracer
The completed Cisco Packet Tracer implementation is located in:

`Packet Tracer/`

### Report
The final Milestone 2 report is located in:

`Report/`

### Documentation
Configuration and testing documentation is located in:

`Documentation/`

### Evidence
Supporting screenshots are located in:

`Evidence/`

---

## Verification

The implementation was tested for:

1. VLAN configuration
2. Core SVI operation
3. Trunking
4. LACP EtherChannel
5. DHCP operation
6. Inter-VLAN routing
7. HTTP server connectivity
8. HTTP access from multiple VLANs
9. Management VLAN connectivity
10. Troubleshooting and recovery

---

## Result

The Milestone 2 implementation was verified using Cisco Packet Tracer
show commands, DHCP lease information, connectivity tests and HTTP
browser tests.

See the final report for the complete testing evidence and results.
