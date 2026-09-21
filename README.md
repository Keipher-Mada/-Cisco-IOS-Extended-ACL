# Cisco Network Security – Extended ACL Firewall Lab

This project demonstrates the implementation of network access controls using
Cisco IOS Extended Access Control Lists (ACLs) in Cisco Packet Tracer.

The objective was to secure communication between a client network and a server
network by applying firewall-style packet filtering at the router.

The lab focuses on the principle of **least privilege**, allowing users and
services only the network access they require while blocking unauthorised
traffic.

## Network Topology

Network topology diagram:
../topology.png

The environment contains two separate IPv4 networks:

- Client LAN: `192.168.1.0/24`
- Server LAN: `192.168.2.0/24`

The networks are connected through a Cisco 2911 router.

### Client Devices

| Device | IP Address | Role |
|---|---|---|
| Bob | 192.168.1.100 | Standard User |
| Alice | 192.168.1.101 | Standard User |
| Admin | 192.168.1.102 | Administrator |
| Kiosk | 192.168.1.103 | Restricted Device |

### Server Devices

| Server | IP Address | Service |
|---|---|---|
| Web/FTP Server | 192.168.2.100 | HTTP / FTP |
| Email Server | 192.168.2.101 | SMTP / POP3 |

## Security Requirements

The router was configured to enforce the following security policy:

- Prevent the Admin workstation from sending or receiving SMTP and POP3 traffic.
- Permit the Admin workstation to access the FTP service on the Web/FTP server.
- Prevent other client devices from accessing FTP services.
- Prevent Bob and Alice from using ICMP to ping the server network.
- Completely prevent the Kiosk device from communicating with the
  `192.168.2.0/24` server network.
- Permit other authorised IP traffic.

## Security Implementation

Extended Cisco IOS ACLs were used because the filtering requirements depend on
multiple packet attributes, including:

- Source IP address
- Destination IP address
- IP protocol
- TCP/UDP service port
- ICMP traffic

The ACL was applied at the router boundary between the user LAN and server LAN,
allowing the router to act as the enforcement point between the two security
zones.

### Example Policy Logic

```text
Kiosk -> Server Network          DENY
Bob -> Servers (ICMP)            DENY
Alice -> Servers (ICMP)          DENY

Admin -> FTP Server (FTP)        PERMIT
Other Clients -> FTP             DENY

Admin -> Email Services          DENY

Other authorised IP traffic      PERMIT

This demonstrates how ACL ordering is important, as Cisco ACLs process rules
from top to bottom until a matching rule is found.

Testing and Validation

After implementing the ACLs, connectivity was tested from each endpoint to
verify that the security policy behaved as expected.

Tests included:

ICMP connectivity between client and server networks
FTP access from the administrator workstation
FTP access attempts from unauthorised workstations
Kiosk access attempts to the server subnet
SMTP and POP3 traffic restrictions
Verification that permitted traffic continued to function

Both successful connections and expected connection failures were used to
validate the ACL configuration.

Security Concepts Demonstrated

This project demonstrates practical understanding of:

Network segmentation
Cisco IOS Extended ACLs
Packet filtering
Firewall rule design
Source and destination-based filtering
Port and protocol filtering
ICMP filtering
Least privilege
Access control policy enforcement
Rule ordering
Network troubleshooting
Security testing and validation
Tools and Technologies
Cisco Packet Tracer
Cisco IOS
IPv4
TCP/IP
Extended Access Control Lists
FTP
HTTP
SMTP
POP3
ICMP
What I Learned

This lab reinforced the importance of designing firewall rules around business
requirements rather than simply allowing or denying entire networks.

It also demonstrated how the placement and ordering of ACL entries can affect
network behaviour. More specific security rules must be evaluated before
broader permit statements to prevent unintended access.

The project provided practical experience translating security requirements
into enforceable network controls and validating those controls through
connectivity testing.
```
