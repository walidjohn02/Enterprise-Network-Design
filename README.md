# Secure & Redundant Enterprise Network Design

## Overview

This project demonstrates the design and implementation of a **secure, redundant multi-VLAN enterprise network** using Cisco technologies. The goal was to build a realistic enterprise topology that emphasizes **high availability, security, and proper Layer 2/Layer 3 design principles**, rather than simple connectivity.

The lab was implemented and tested using **Cisco Packet Tracer**.

---

## Network Architecture

### Topology Components

* 2 × Layer 2 Access Switches (SW1, SW2)
* 2 × Routers (R1, R2) using Router-on-a-Stick
* End devices distributed across multiple VLANs
* Simulated Internet connection using NAT

### VLAN & IP Design

| VLAN | Purpose | Subnet          | Default Gateway (HSRP VIP) |
| ---- | ------- | --------------- | -------------------------- |
| 10   | Users   | 192.168.10.0/24 | 192.168.10.1               |
| 20   | Admins  | 192.168.20.0/24 | 192.168.20.1               |
| 30   | Servers | 192.168.30.0/24 | 192.168.30.1               |

* `.2` and `.3` are statically assigned to routers
* DHCP leases start from `.4`

---

## High Availability Design

### HSRP (Hot Standby Router Protocol)

* Implemented on all VLANs
* R1 configured as Active router (higher priority)
* R2 configured as Standby router
* Preemption enabled to ensure deterministic failover

**Purpose:** Maintain uninterrupted default gateway availability during router failures.

---

## Security Features Implemented

### Port Security

* Enabled on all access ports
* Sticky MAC address learning
* Violation mode: restrict

**Threat Mitigated:** Unauthorized device access

---

### DHCP Snooping

* Enabled on VLANs 10, 20, and 30
* Trusted only on infrastructure trunk links
* DHCP Option 82 disabled

**Threat Mitigated:** Rogue DHCP servers

---

### Dynamic ARP Inspection (DAI)

* Enabled on all user VLANs
* Trusted only on switch-to-switch and router uplinks
* Uses DHCP Snooping bindings for validation

**Threat Mitigated:** ARP spoofing and man-in-the-middle attacks

---

### Access Control Lists (ACLs)

* Extended ACL used to restrict HTTP access from User VLAN to Server VLAN
* Applied closest to the source

**Purpose:** Enforce inter-VLAN security policies

---

## Routing & NAT

* Inter-VLAN routing implemented via router sub-interfaces
* Static default route configured toward ISP
* NAT overload (PAT) configured on edge router

**Purpose:** Enable internet access while preserving private IP addressing internally

---

## Testing & Validation

### Failure & Security Testing Performed

| Test                  | Action                      | Result                             |
| --------------------- | --------------------------- | ---------------------------------- |
| Router failure        | Shut down R1 interface      | Traffic failed over to R2 via HSRP |
| Switch uplink failure | Disabled SW1 trunk          | STP reconverged successfully       |
| Rogue DHCP            | Introduced fake DHCP server | DHCP Snooping blocked offers       |
| ARP spoofing          | Injected fake ARP replies   | DAI dropped malicious packets      |

---

## Design Decisions & Best Practices

* VLAN segmentation reduces broadcast domains and improves security
* HSRP provides gateway-level redundancy independent of routing
* STP root placement aligned with HSRP Active routers to optimize traffic paths
* Security features implemented at Layer 2 to stop attacks as early as possible

---

## Skills Demonstrated

* Enterprise network design
* Redundancy and failover planning
* Layer 2 security hardening
* Troubleshooting and validation
* Clear technical documentation

---

## Tools & Technologies

* Cisco IOS
* VLANs & Trunking
* HSRP
* STP (PVST)
* DHCP Snooping
* Dynamic ARP Inspection
* Port Security
* ACLs
* NAT
* Cisco Packet Tracer

---

## Notes

This project is intended to demonstrate practical networking knowledge and design thinking suitable for junior network, cloud, or infrastructure roles.

Further enhancements could include:

* Network automation (Ansible)
* Monitoring and syslog integration
* Cloud network mapping (AWS VPC equivalent)
