# Server0 setup (Packet Tracer GUI)

Server0 is connected to `SW1 FastEthernet0/2` on VLAN 20. Give it a **static** address so the router's DHCP service never assigns that address to a client.

## Desktop > IP Configuration

| Setting | Value |
|---|---|
| IP address | `192.168.20.10` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `192.168.20.1` |
| DNS server | `192.168.20.10` |

## Services > HTTP

Turn **HTTP On**. The default Packet Tracer web page is sufficient for the test.

## Services > DNS

Turn **DNS On** and add this A record:

| Name | Address |
|---|---|
| `www.office.local` | `192.168.20.10` |

Use the **Save** control if shown. DHCP is provided by R1, not by Server0.

## Guest-access design note

The Guest ACL permits TCP/80 and TCP/UDP port 53 only to `192.168.20.10`, then denies other Guest traffic to VLANs 10 and 20. As a result, Guest web/DNS tests should pass while Guest ping/SSH attempts to Staff and IT devices should fail.
