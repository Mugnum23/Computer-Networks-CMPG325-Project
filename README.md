# Tau Metalworks & Engineering Supplies - Network Design and Implementation
CMPG 325 (Computer Networks) Individual Semester Project - North-West University, 2026.

This repository contains the design proposal, Packet Tracer implementation, configuration files, verification evidence, and supporting documentation for the enterprise network developed for **Tau Metalworks & Engineering Supplies**, a manufacturing organisation based in Potchefstroom.

The project demonstrates an end-to-end engineering workflow: progressing from initial client requirements and architectural design to full implementation, change management (CR2), and verification in Cisco Packet Tracer.

---

## Project Information

| Field | Detail |
|---|---|
| **Student** | T. Magwatane (42710677) |
| **Module** | CMPG 325 - Computer Networks |
| **Project ID** | CMPG325-2026-034 |
| **Client ID** | CLI-034 |
| **Assigned Organisation** | Tau Metalworks & Engineering Supplies |
| **Location** | Potchefstroom, South Africa |
| **Industry** | Manufacturing |
| **Assigned Addressing Block** | `172.30.16.0/23` |
| **Assigned Networking Challenge** | HTTP/Web Server — Internal Web Service Hosting |
| **Design Constraint** | Critical services must remain available during business hours |
| **Client Change Request** | CR2 — Additional floor/area incorporated into the network |
| **Implementation Platform** | Cisco Packet Tracer (v8.x+) |
| **Project Period** | 14 August 2026 – 16 October 2026 |

---

## Project Milestones

| Date | Milestone | Status |
|---|---|---|
| 28 August 2026 | Milestone 1 - Client Design Review | Completed |
| 02 October 2026 | Milestone 2 - Client Implementation Review | Completed |
| 16 October 2026 | Final Submission | Pending |

---

## Repository Structure

```text
tau-metalworks-network-cmpg325-2026034/
│
├── README.md
├── technical-report.pdf
│
├── 01-requirements/
│   └── client-requirements.md
│
├── 02-design/
│   ├── physical-topology.png
│   ├── logical-topology.png
│   └── design-decisions.md
│
├── 03-ip-addressing/
│   └── ip-addressing-plan.md
│
├── 04-configuration/
│   └── device-configs/        # Exported running-configuration files per device
│
├── 05-testing/
│   ├── testing-log.md
│   └── screenshots/
│
├── 06-troubleshooting/
│   └── issues-and-fixes.md
│
├── 07-change-request-CR2/
│   └── cr2-implementation.md
│
├── packet-tracer/
│   └── tau-metalworks-network.pkt
│
└── Milestone 2/               # Milestone 2 submission package & artifacts
    ├── README.md
    ├── Packet Tracer/
    ├── Report/
    ├── Documentation/
    ├── Evidence/
