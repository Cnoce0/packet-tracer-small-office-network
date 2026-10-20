# Verification test plan

Run these checks after building the topology and applying configurations. Record the actual outcome in `test-results.csv` and keep screenshots in `screenshots/`.

| ID | Test | Command / method | Expected result |
|---|---|---|---|
| T-01 | VLAN configuration | SW1: `show vlan brief` | VLANs 10, 20, 30, and 999 exist; access ports are assigned as documented |
| T-02 | Trunk | SW1: `show interfaces trunk` | Fa0/1 is trunking and allows VLANs 10, 20, 30 |
| T-03 | Router subinterfaces | R1: `show ip interface brief` | G0/0 and G0/0.10/.20/.30 are up/up after physical links come up |
| T-04 | DHCP leases | R1: `show ip dhcp binding`; each PC: `ipconfig` | PC1/PC2 get 192.168.10.x, PC3 gets 192.168.20.x, PC4 gets 192.168.30.x; dynamic last octet between 50 and 200 |
| T-05 | Staff to IT reachability | PC1 command prompt: `ping <PC3-address>` | Replies received |
| T-06 | Staff to server | PC1: `ping 192.168.20.10` | Replies received |
| T-07 | Guest isolation | PC4: `ping <PC1-address>` and `ping <PC3-address>` | Requests time out / blocked |
| T-08 | Guest HTTP + DNS | PC4 browser: `http://www.office.local` | Server0 web page loads; DNS resolves name to 192.168.20.10 |
| T-09 | Router SSH | PC3 command prompt: `ssh -l netadmin 192.168.20.1` | SSH login succeeds using the lab-only password |
| T-10 | Switch SSH | PC3 command prompt: `ssh -l netadmin 192.168.20.2` | SSH login succeeds using the lab-only password |
| T-11 | Port security | SW1: `show port-security`; `show port-security interface fa0/3` | Port security enabled on endpoint ports; maximum 1 secure MAC |
| T-12 | Unused ports | SW1: `show interfaces status` | Fa0/7–24 and Gi0/1–2 are disabled |
| T-13 | ACL | R1: `show access-lists GUEST-IN` | Guest HTTP/DNS permits and isolation deny entries present; counters increase during tests |

## Important test interpretation

Guest ICMP to Server0 is expected to be denied because the ACL only explicitly permits DNS and HTTP to the server. “Guest reaches the server” means the web page loads by DNS name, not that ping succeeds. Router and switch SSH tests require supported Packet Tracer IOS/client features. If a PC SSH client behaves differently in the selected Packet Tracer release, verify from another supported terminal/client and document the difference.
