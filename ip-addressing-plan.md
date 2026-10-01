# IP Addressing Plan

**Assigned block:** 172.30.30.0/23 (172.30.30.0 – 172.30.31.255, 512 addresses)

Subnetted into eight /26 blocks (increment of 64). Five are allocated to
VLANs now; three are reserved for future growth.

| VLAN | Purpose | Subnet | Mask | Usable range | Broadcast | Gateway |
|---|---|---|---|---|---|---|
| 10 | Administration / Events Booking | 172.30.30.0/26 | 255.255.255.192 | 172.30.30.1 – 172.30.30.62 | 172.30.30.63 | 172.30.30.1 |
| 20 | Point-of-Sale (resilient segment) | 172.30.30.64/26 | 255.255.255.192 | 172.30.30.65 – 172.30.30.126 | 172.30.30.127 | 172.30.30.65 |
| 30 | Servers (Web/HTTP) | 172.30.30.128/26 | 255.255.255.192 | 172.30.30.129 – 172.30.30.190 | 172.30.30.191 | 172.30.30.129 |
| 40 | Staff Wireless | 172.30.30.192/26 | 255.255.255.192 | 172.30.30.193 – 172.30.30.254 | 172.30.30.255 | 172.30.30.193 |
| 50 | Contractor Wireless (limited, CR14) | 172.30.31.0/26 | 255.255.255.192 | 172.30.31.1 – 172.30.31.62 | 172.30.31.63 | 172.30.31.1 |
| — | Reserved | 172.30.31.64/26 | 255.255.255.192 | — | 172.30.31.127 | — |
| — | Reserved | 172.30.31.128/26 | 255.255.255.192 | — | 172.30.31.191 | — |
| — | Reserved | 172.30.31.192/26 | 255.255.255.192 | — | 172.30.31.255 | — |

## Notes

- Router0 uses router-on-a-stick with one sub-interface per VLAN (or a
  Layer 3 switch with SVIs, depending on the final device choice), each
  configured with the gateway address above.
- The PoS VLAN (20) and Server VLAN (30) sit on a switch with an
  independent uplink to the router, separate from the office switch
  carrying VLANs 10, 40, and 50 — this is how the design constraint
  (PoS must stay online if the office LAN fails) is met.
- The Contractor VLAN (50) should carry an ACL restricting it from
  reaching VLANs 10, 20, and 30, satisfying CR14's "limited access"
  requirement.
- DHCP scopes for VLANs 10, 40, and 50 will be configured on the router;
  PoS (VLAN 20) and servers (VLAN 30) should use static addressing for
  reliability.
