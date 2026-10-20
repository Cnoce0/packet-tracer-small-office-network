# Build the topology in Cisco Packet Tracer

This repository contains the IOS configuration text and a topology plan. A native `.pkt` file must be saved from Cisco Packet Tracer after you create the devices; this repository does not pretend to contain a generated or tested `.pkt` file.

## 1. Place devices

- 1 × Router-PT using the Cisco 2911 model, named `R1`
- 1 × Switch-PT using the Cisco 2960 model, named `SW1`
- 1 × Server-PT, named `Server0`
- 4 × PC-PT, named `PC1`, `PC2`, `PC3`, and `PC4`

## 2. Cable them

Use Copper Straight-Through cables for each link:

| From | To | Purpose |
|---|---|---|
| `R1 GigabitEthernet0/0` | `SW1 FastEthernet0/1` | 802.1Q trunk; router-on-a-stick |
| `Server0 FastEthernet0` | `SW1 FastEthernet0/2` | Server, VLAN 20 |
| `PC1 FastEthernet0` | `SW1 FastEthernet0/3` | Staff, VLAN 10 |
| `PC2 FastEthernet0` | `SW1 FastEthernet0/4` | Staff, VLAN 10 |
| `PC3 FastEthernet0` | `SW1 FastEthernet0/5` | IT, VLAN 20 |
| `PC4 FastEthernet0` | `SW1 FastEthernet0/6` | Guest, VLAN 30 |

Wait for links to turn green. If a link remains down, confirm the correct interfaces and `no shutdown` on R1's `GigabitEthernet0/0`.

## 3. Configure R1 and SW1

1. Open R1 > **CLI**. Complete the initial dialog if prompted, then paste the contents of `configs/R1.txt` into the CLI. If Packet Tracer's IOS rejects the inline RSA key-generation syntax or does not create an SSH key, enter global configuration mode, set `ip domain-name office.local`, then run `crypto key generate rsa` and answer the modulus prompt with `1024`. Reapply `ip ssh version 2` afterward.
2. Open SW1 > **CLI** and paste `configs/SW1.txt`.
3. If Packet Tracer prompts for initial configuration, answer **no** and press Enter to reach the CLI first.
4. If a device reports a specific unsupported command, remove only that unsupported line, continue the configuration, and record the version/command in the README before claiming the feature is verified.

## 4. Configure Server0 and PCs

- Configure Server0 using `configs/server-setup.md`.
- On each PC, open **Desktop > IP Configuration > DHCP**.
- Expected DHCP leases are in the `.50`–`.200` range for that PC's VLAN, with the VLAN gateway ending in `.1` and DNS server `192.168.20.10`.

## 5. Save the native project

Choose **File > Save As** and save the Packet Tracer file as `small-office-network.pkt` in the repository root. Reopen that saved file once to ensure it loads. The `.pkt` should be committed only after you have created and checked it in Packet Tracer.

## 6. Capture evidence

Take genuine screenshots from Packet Tracer and save them in `screenshots/`, e.g. `topology.png`, `dhcp-pc1.png`, `staff-to-it-ping.png`, `guest-http.png`, `guest-blocked-ping.png`, and `ssh-r1.png`. Do not mark a test as passed until it has actually been run.
