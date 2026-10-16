# IP Addressing Plan — Tau Metalworks & Engineering Supplies

## 1. Project Information

| Item                 | Details                               |
| -------------------- | ------------------------------------- |
| Project              | CMPG325 — Computer Networks           |
| Project ID           | CMPG325-2026-034                      |
| Client ID            | CLI-034                               |
| Client location      | Potchefstroom, South Africa           |
| Allocated IPv4 block | `172.30.16.0/23`                      |
| Addressing method    | Variable Length Subnet Masking (VLSM) |
| Routing device       | `CORE-SWITCH`                         |
| Management VLAN      | VLAN 99                               |

## 2. Addressing Design Overview

The allocated block `172.30.16.0/23` contains 512 IPv4 addresses (`172.30.16.0 – 172.30.17.255`). VLSM divides it into subnets sized to each department, the server network, the CR2 floor and network management. Each VLAN has a dedicated subnet and a default gateway configured as a Switched Virtual Interface (SVI) on the Layer-3 core switch, which also provides inter-VLAN routing.

## 3. VLAN and Subnet Allocation

| VLAN | Purpose         | Network / Prefix   | Subnet Mask       | Usable Host Range             | Broadcast       | Default Gateway            |
| ---- | --------------- | ------------------ | ----------------- | ----------------------------- | --------------- | -------------------------- |
| 10   | Administration  | `172.30.16.0/26`   | `255.255.255.192` | `172.30.16.1–172.30.16.62`    | `172.30.16.63`  | `172.30.16.1`              |
| 20   | Sales           | `172.30.16.64/26`  | `255.255.255.192` | `172.30.16.65–172.30.16.126`  | `172.30.16.127` | `172.30.16.65`             |
| 30   | Engineering     | `172.30.16.128/26` | `255.255.255.192` | `172.30.16.129–172.30.16.190` | `172.30.16.191` | `172.30.16.129`            |
| 40   | Production      | `172.30.16.192/26` | `255.255.255.192` | `172.30.16.193–172.30.16.254` | `172.30.16.255` | `172.30.16.193`            |
| 50   | Warehouse       | `172.30.17.0/26`   | `255.255.255.192` | `172.30.17.1–172.30.17.62`    | `172.30.17.63`  | `172.30.17.1`              |
| 60   | Servers         | `172.30.17.64/27`  | `255.255.255.224` | `172.30.17.65–172.30.17.94`   | `172.30.17.95`  | `172.30.17.65`             |
| 70   | CR2 / New Floor | `172.30.17.96/27`  | `255.255.255.224` | `172.30.17.97–172.30.17.126`  | `172.30.17.127` | `172.30.17.97`             |
| 99   | Management      | `172.30.17.224/28` | `255.255.255.240` | `172.30.17.225–172.30.17.238` | `172.30.17.239` | `172.30.17.225` (core SVI) |

## 4. Static IP Address Assignments

| Device                         | VLAN | IP Address / Prefix | Default Gateway                         |
| ------------------------------ | ---- | ------------------- | --------------------------------------- |
| `SRV-HTTP`                     | 60   | `172.30.17.66/27`   | `172.30.17.65`                          |
| `CORE-SWITCH` (management SVI) | 99   | `172.30.17.225/28`  | Not applicable (directly connected SVI) |
| `SW-GROUND`                    | 99   | `172.30.17.226/28`  | `172.30.17.225`                         |
| `SW-ENG`                       | 99   | `172.30.17.227/28`  | `172.30.17.225`                         |
| `SW-PROD`                      | 99   | `172.30.17.228/28`  | `172.30.17.225`                         |
| `SW-WARE`                      | 99   | `172.30.17.229/28`  | `172.30.17.225`                         |
| `SW-SERVER`                    | 99   | `172.30.17.230/28`  | `172.30.17.225`                         |
| `SW-NEWFLOOR`                  | 99   | `172.30.17.231/28`  | `172.30.17.225`                         |

The HTTP server is statically addressed to provide a consistent destination for HTTP tests. Switch management addresses sit in the dedicated VLAN 99 subnet, and the core's VLAN 99 SVI is the gateway for that subnet.

## 5. DHCP Address Allocation

DHCP is configured on `CORE-SWITCH` for the client VLANs below. VLAN 60 (static server) and VLAN 99 (static management) are not DHCP-served.

| VLAN | Purpose         | Subnet             | Gateway         | Lease observed in testing        |
| ---- | --------------- | ------------------ | --------------- | -------------------------------- |
| 10   | Administration  | `172.30.16.0/26`   | `172.30.16.1`   | `172.30.16.2` (PC-ADMIN)         |
| 20   | Sales           | `172.30.16.64/26`  | `172.30.16.65`  | `172.30.16.66` (PC-SALES)        |
| 30   | Engineering     | `172.30.16.128/26` | `172.30.16.129` | `172.30.16.130` (PC-ENGINEERING) |
| 40   | Production      | `172.30.16.192/26` | `172.30.16.193` | `172.30.16.194` (PC-PRODUCTION)  |
| 50   | Warehouse       | `172.30.17.0/26`   | `172.30.17.1`   | `172.30.17.2` (PC-WAREHOUSE)     |
| 70   | CR2 / New Floor | `172.30.17.96/27`  | `172.30.17.97`  | `172.30.17.98` (PC-CR2)          |

###### 5.1 DHCP Exclusions

The current core-switch configuration excludes the default gateway address from each DHCP-served client subnet:

- VLAN 10: `172.30.16.1`
- VLAN 20: `172.30.16.65`
- VLAN 30: `172.30.16.129`
- VLAN 40: `172.30.16.193`
- VLAN 50: `172.30.17.1`
- VLAN 70: `172.30.17.97`

The configuration also excludes `172.30.17.65–172.30.17.70` in the server subnet. VLAN 60 is intended for static server addressing.

## 6. Future Address Allocation

`172.30.17.128 – 172.30.17.223` is unallocated and reserved for growth (additional departments, segments or services). Any future subnet must be checked against the existing scheme to prevent overlap.

## 7. Addressing Principles

1. Each VLAN uses a separate IP subnet.
2. Each VLAN has its gateway on `CORE-SWITCH`, which performs inter-VLAN routing.
3. Client VLANs use DHCP; the HTTP server and management interfaces use static addresses.
4. Management traffic uses its own subnet (VLAN 99).
5. CR2 uses VLAN 70 and a dedicated /27, so it was added without changing any existing subnet.
6. Reserved space allows future expansion.

## 8. Verification

| Command                   | Purpose                            |
| ------------------------- | ---------------------------------- |
| `show ip interface brief` | SVI addresses and status           |
| `show ip route`           | Connected routes for each subnet   |
| `show ip dhcp pool`       | DHCP pool configuration            |
| `show ip dhcp binding`    | Client leases                      |
| `show vlan brief`         | VLAN creation and port assignments |

Results and screenshots are recorded in `05-testing/testing-log.md`.

## 9. Conclusion

The `172.30.16.0/23` block is divided into eight subnets for the Administration, Sales, Engineering, Production, Warehouse, Server, CR2 and Management VLANs. VLSM allocates addresses efficiently, VLANs provide logical separation, inter-VLAN routing connects the segments, and the reserved range supports future expansion.
