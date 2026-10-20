# Troubleshooting lab tickets

These are four reproducible fault-injection exercises. They are **scenarios to perform**, not a claim that they have already been run. Start from the known-good configuration, introduce one fault, observe it, collect command output, then apply the fix and rerun the relevant test. Save before/after screenshots with the ticket ID in the filename.

## TK-001 — Staff PC receives a Guest address

- **User report:** “PC1 is getting an address in the wrong network and cannot use staff resources.”
- **Introduce:** On SW1, run `configure terminal`, `interface FastEthernet0/3`, `switchport access vlan 30`.
- **Observe / diagnose:** On PC1 run `ipconfig`, then R1 `show ip dhcp binding`; on SW1 run `show vlan brief`.
- **Root cause:** PC1's switch access port is assigned to VLAN 30 instead of VLAN 10.
- **Fix:** On SW1: `configure terminal` → `interface FastEthernet0/3` → `switchport access vlan 10` → `end`.
- **Verify:** On PC1 select DHCP again / renew if available. It should receive `192.168.10.x`; confirm staff-to-IT ping.
- **Evidence filenames:** `tk001-before.png`, `tk001-after.png`.

## TK-002 — Staff clients receive the wrong default gateway

- **User report:** “My PC has an IP address but cannot reach other subnets.”
- **Introduce:** On R1, `configure terminal` → `ip dhcp pool STAFF-VLAN10` → `default-router 192.168.10.254` → `end`. For a quick reproducible test, release/renew or toggle PC1's DHCP selection. Existing leases may retain old options until renewed.
- **Observe / diagnose:** PC1 `ipconfig`; try `ping 192.168.10.254` and `ping 192.168.10.1`. Check `show running-config | section ip dhcp pool` on R1.
- **Root cause:** The DHCP pool advertises a nonexistent gateway (`192.168.10.254`) instead of R1's VLAN 10 address.
- **Fix:** On R1: `configure terminal` → `ip dhcp pool STAFF-VLAN10` → `default-router 192.168.10.1` → `end`.
- **Verify:** Renew PC1's lease, confirm gateway is `192.168.10.1`, and ping the IT PC.
- **Evidence filenames:** `tk002-before.png`, `tk002-after.png`.

## TK-003 — Guest web access breaks because the ACL is too restrictive

- **User report:** “The Guest Wi-Fi/guest PC cannot open the office information page.”
- **Introduce:** In R1's `GUEST-IN` ACL, remove the two DNS permits and HTTP permit or place the `deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255` entry above them. Rebuilding the named ACL is the simplest controlled fault-injection method; retain a copy of the known-good ACL first.
- **Observe / diagnose:** From PC4 open `http://www.office.local`; on R1 run `show access-lists GUEST-IN` and inspect rule order and counters.
- **Root cause:** A deny entry is evaluated before the DNS/HTTP exceptions, so packets to Server0 are dropped before they can match a permit.
- **Fix:** Restore the `GUEST-IN` ACL in `configs/R1.txt` in its listed order: DNS UDP/TCP permits, HTTP permit, Guest-to-Staff deny, Guest-to-IT deny, then final Guest-to-any permit.
- **Verify:** From PC4 browse to `http://www.office.local` (should load), while ping to PC1/PC3 remains blocked.
- **Evidence filenames:** `tk003-before.png`, `tk003-after.png`.

## TK-004 — Guest VLAN missing from the trunk allow list

- **User report:** “The guest computer cannot get an IP address; staff is still working.”
- **Introduce:** On SW1: `configure terminal` → `interface FastEthernet0/1` → `switchport trunk allowed vlan 10,20` → `end`.
- **Observe / diagnose:** On SW1 run `show interfaces trunk` and `show vlan brief`; on PC4 run `ipconfig` and try DHCP again. R1's Guest subinterface may still be configured, but VLAN 30 frames are not carried over the trunk.
- **Root cause:** VLAN 30 was omitted from the switch-to-router trunk's allowed VLAN list.
- **Fix:** On SW1: `configure terminal` → `interface FastEthernet0/1` → `switchport trunk allowed vlan 10,20,30` → `end`.
- **Verify:** Renew PC4's DHCP lease and confirm it receives `192.168.30.x`; verify Guest isolation and HTTP access again.
- **Evidence filenames:** `tk004-before.png`, `tk004-after.png`.

## Ticket discipline

For each exercise, record the actual timestamp, commands/output, result, and a brief note about what eliminated competing causes. Don't write “verified” until the after-test has really passed in Packet Tracer.
