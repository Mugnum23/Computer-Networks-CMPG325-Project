# Tau Metalworks & Engineering Supplies — Enterprise Network Design & Implementation
**Module:** CMPG 325 (Computer Networks) Individual Semester Project  
**Institution:** North-West University  
**Academic Year:** 2026  

---

## Executive Summary

This repository contains the end-to-end design, Packet Tracer implementation, device configurations, testing logs, and change management documentation for the enterprise network of **Tau Metalworks & Engineering Supplies**, a manufacturing organisation based in Potchefstroom, South Africa.

The network architecture addresses key business requirements, including departmental segmentation, centralized dynamic addressing, high-availability server access, secure administrative access, internal web hosting, and dynamic scaling to accommodate physical site expansions (Change Request CR2).

---

## Project Specifications

| Specification Field | Project Detail |
|---|---|
| **Student Name & ID** | T. Magwatane (42710677) |
| **Module Code** | CMPG 325 — Computer Networks |
| **Project Identifier** | CMPG325-2026-034 |
| **Client ID** | CLI-034 |
| **Assigned Organisation** | Tau Metalworks & Engineering Supplies |
| **Location** | Potchefstroom, South Africa |
| **Industry Sector** | Manufacturing |
| **Assigned IP Block** | `172.30.16.0/23` (`172.30.16.0` – `172.30.17.255`) |
| **Networking Challenge** | HTTP/Web Server — Internal Web Service Hosting |
| **Business Constraint** | High Availability: Critical services must remain accessible during business hours |
| **Change Request** | CR2 — Integration of an additional floor/building expansion |
| **Simulation Platform** | Cisco Packet Tracer (v8.0+) |
| **Project Timeline** | 14 August 2026 – 16 October 2026 |

---

## Project Milestones & Deliverables

| Milestone | Target Date | Description | Status |
|---|---|---|---|
| **Milestone 1** | 28 August 2026 | Client Requirements Analysis & Network Design Proposal | Completed |
| **Milestone 2** | 02 October 2026 | Cisco Packet Tracer Implementation, SVI Routing & Verification | Completed |
| **Final Submission**| 16 October 2026 | Final Technical Report, Evidence Package & Repository Review | Pending |

---

## Detailed Network Topologies

### 1. Physical Topology Structure

The physical layout adopts a **two-tier extended-star architecture** centered in the primary distribution frame (MDF) on the Ground Floor, linking intermediate distribution frames (IDFs) on upper floors via redundant high-speed uplinks.

```text
                                [ Layer 3 Core Switch ]
                                   (DS-CORE-01)
                                        │
     ┌──────────────┬──────────────┬────┴─────────┬──────────────┬──────────────┐
     │ 802.1Q       │ 802.1Q       │ 802.1Q       │ 802.1Q       │ LACP         │ 802.1Q
     │ Trunk        │ Trunk        │ Trunk        │ Trunk        │ EtherChannel │ Trunk
     ▼              ▼              ▼              ▼              ▼              ▼
[ SW-GF-ADMIN ] [ SW-FL1-SALES ] [ SW-FL1-ENG ] [ SW-FL2-PROD ] [ SW-SERVER ]  [ SW-NEWFLOOR ]
 (Ground Fl.)    (1st Floor)     (1st Floor)    (2nd Floor)    (Server Room)  (CR2 Expansion)
     │              │              │              │              │              │
     ├─ PC-Admin-01 ├─ PC-Sales-01 ├─ PC-Eng-01   ├─ PC-Prod-01  ├─ HTTP-01     └─ PC-CR2-01
     └─ PC-Admin-02 └─ PC-Sales-02 └─ PC-Eng-02   └─ PC-Prod-02  └─ DHCP-Pools     └─ PC-CR2-02
