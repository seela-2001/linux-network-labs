---
tags: [troubleshooting, linux, networking, netplan]
date: 2026-10-07
lab: "[[Lab - Static IP with nmcli]]"
---

# NetworkManager Connection Storage with Netplan

## Symptom
`/etc/NetworkManager/system-connections/` was empty even though the
connection `static-lab` existed.

## Troubleshooting
- Checked `/run/NetworkManager/system-connections/`: many files named
  `netplan-NM-<UUID>.nmconnection`.
- `ls -l | grep static` found nothing because files are named by **UUID**.
- Matched the UUID printed at creation time
  (`8af6f14a-84a2-4fdf-8c5b-5cdcc5b761f0`) with the file name.

## Cause
NetworkManager is integrated with netplan on this system. The file in `/run`
is a generated copy (lost on reboot). The persistent config is in
`/etc/netplan/`.

## Where things are
| Purpose | Path |
|---|---|
| Runtime copy (generated) | `/run/NetworkManager/system-connections/netplan-NM-<UUID>.nmconnection` |
| Persistent config | `/etc/netplan/90-NM-<UUID>.yaml` |

## Useful commands
```bash
nmcli -f NAME,UUID,TYPE,FILENAME con show
sudo ls -l /etc/netplan/
sudo cat /etc/netplan/90-NM-<UUID>.yaml
```

## Rule
Don't edit files in `/run`. Use `nmcli con mod ...`, or edit the YAML in
`/etc/netplan/` and run `sudo netplan apply`.

See also: [[Netplan and NetworkManager]] | Back to: [[Lab - Static IP with nmcli]]
