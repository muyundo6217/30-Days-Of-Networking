# Day 1 — Cisco IOS Navigation & Basic Device Setup

## Lab Overview (Used Claude to clean up the document)
Configured basic device settings on a Cisco router and switch including
hostname, passwords, banner, IP addressing, and management access.
Practiced navigating all IOS modes and saving configuration.

---

## Topology
```
[PC1] --- [SW1] --- [R1]
192.168.1.10    192.168.1.2    192.168.1.1
```

---

## IP Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|------------|-------------|-----------------|
| R1 | Gi0/0 | 192.168.1.1 | 255.255.255.0 | — |
| SW1 | VLAN 1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC1 | NIC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |

---

## Lab Objectives
- Navigate User EXEC, Privileged EXEC, and Global Config modes
- Configure hostname, enable secret, and console password
- Set a MOTD banner on R1 and SW1
- Assign IP address to R1 Gi0/0 and SW1 VLAN 1
- Save configuration using write memory
- Verify with show running-config and show ip interface brief

---

## Configuration Applied

### R1 Configuration
```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret cisco
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit
R1(config)# banner motd #
WARNING: Authorized Access Only!
#
R1(config)# interface gi0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# end
R1# write memory
```

### SW1 Configuration
```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# enable secret cisco
SW1(config)# line console 0
SW1(config-line)# password cisco
SW1(config-line)# login
SW1(config-line)# exit
SW1(config)# banner motd #
WARNING: Authorized Access Only!
#
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# ip default-gateway 192.168.1.1
SW1(config)# end
SW1# write memory
```

---

## Verification

### show ip interface brief — R1
```
Interface              IP-Address      OK? Method Status    Protocol
GigabitEthernet0/0    192.168.1.1     YES manual up        up
GigabitEthernet0/1    unassigned      YES unset  admin down down
```

### Ping Test — R1 to SW1
```
R1# ping 192.168.1.2
!!!!!
Success rate is 100 percent (5/5)
```

---

## What I Learned

**IOS Mode Navigation:**
Every Cisco device has three main modes you move between constantly.
User EXEC (>) gives you read-only access. Privileged EXEC (#) gives
you full access to show commands and file management. Global Config
(config)# is where you make changes to the device.

**Why the MOTD Banner Matters:**
The banner is not just cosmetic — it serves as a legal warning that
unauthorized access is prohibited. In a real NOC environment every
device should have one. Without it, prosecuting unauthorized access
becomes legally complicated in some jurisdictions.

**Interface Status Codes:**
up/up means the interface is fully working. admin down means someone
ran the shutdown command. Knowing these instantly is critical for
NOC troubleshooting — show ip interface brief is the first command
you run on any ticket.

---

## Challenges Faced
<!-- Fill this in with any issues you ran into and how you solved them -->
- remembering to assign an ip address and default gateway to the main PC

---

## Commands Reference

| Command | What It Does |
|---------|-------------|
| `enable` | Enter privileged EXEC mode |
| `configure terminal` | Enter global config mode |
| `hostname [name]` | Set device hostname |
| `enable secret [pass]` | Set encrypted enable password |
| `banner motd #[message]#` | Set login warning message |
| `ip address [ip] [mask]` | Assign IP to interface |
| `no shutdown` | Bring interface up |
| `write memory` | Save running config to startup |
| `show ip interface brief` | View all interface statuses |
| `show running-config` | View current configuration |

---

## NOC Real-World Connection
On my first day at the NOC I will SSH into devices I have never seen
before. Knowing how to navigate IOS modes instantly, read interface
statuses, and verify connectivity without hesitation is non-negotiable.
This lab builds that muscle memory.

---

## Next Lab
[Day 2 — IP Addressing & Interface Configuration](../Day-02-IP-Addressing/)

---

*Part of my 30 Days of Cisco — NOC Technician to Network Engineer series*
*Studying for CCNA | Currently working as NOC Technician*
