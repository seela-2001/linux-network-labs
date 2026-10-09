---
tags: [lab, linux, networking, nmcli]
date: 2026-10-07
status: done
---

# Lab: Static IP with nmcli

## Goal
Create a static IP connection (`192.168.1.50/24`) using NetworkManager.

## Commands
```bash
sudo nmcli con add con-name static-lab ifname eno1 type ethernet \
  ipv4.method manual ipv4.address 192.168.1.50/24
sudo nmcli con mod static-lab ipv4.dns 8.8.8.8
sudo nmcli con up static-lab
```

## Problems I hit
- [[Wrong Interface Name in nmcli]]
- [[NetworkManager Connection Storage with Netplan]]

## Related
- [[Netplan and NetworkManager]]

## Lessons learned
1. Read the error message fully, it usually contains the answer.
2. Verify interface names with `nmcli device status` before creating a profile.
3. Files are named by UUID, so search by UUID, not by connection name.

