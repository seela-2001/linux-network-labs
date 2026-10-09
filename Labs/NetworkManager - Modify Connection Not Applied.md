---
tags: [lab, linux, networking, networkmanager, nmcli]
date: 2026-10-09
status: solved
---

# Lab: nmcli `con mod` changed the profile but not the live IP

Related: [[nmcli con mod does not apply live config]] (troubleshooting)

## Goal
Create a dummy connection, modify its IP, and understand why the interface keeps the old IP until the change is applied.

## Setup
```bash
nmcli con add type dummy ifname dummy0 con-name lab-static \
  ipv4.method manual ipv4.addresses 192.168.50.10/24 ipv6.method disabled
nmcli con up lab-static
ip -br a show dummy0
```
Result: `dummy0  UNKNOWN  192.168.50.10/24`

> `UNKNOWN` state is normal for a dummy interface (no real link/carrier).

## The Challenge (the "break")
A base64-encoded command was run without decoding it first:

```bash
echo 'bm1jbGkgY29uIG1vZCBsYWItc3RhdGljIGlwdjQuYWRkcmVzc2VzIDE5Mi4xNjguNTAuMjAvMjQ=' | base64 -d | bash
```

Decoded (what it actually runs):
```bash
nmcli con mod lab-static ipv4.addresses 192.168.50.20/24
```

## Symptom
Expected `192.168.50.20`, but `ip -br a show dummy0` still showed `192.168.50.10/24`.

## Root cause
`nmcli con mod` edits the **saved profile** only. The **live state** on the interface (kernel) is untouched until the connection is re-activated or re-applied.

| Layer | What it shows | Command |
|---|---|---|
| Profile (NM config) | `.20` right after `mod` | `nmcli -g ipv4.addresses con show lab-static` |
| Live (kernel) | still `.10` | `ip -br a show dummy0` |

## Fix
```bash
nmcli con up lab-static          # re-activate with the new profile
ip -br a show dummy0             # -> 192.168.50.20/24
```
Alternative without bouncing the connection:
```bash
nmcli dev reapply dummy0
```
Note: I ran `reapply` after `con up`, so the IP was already `.20`. To test `reapply` on its own, run `con mod` again with a new IP, then `reapply` instead of `con up`.

Connection was never deleted.

## Key takeaways
- `nmcli con mod` = edit the profile. `nmcli con up` / `nmcli dev reapply` = apply it.
- Always verify the live state with `ip`, not just `nmcli con show`.
- Never pipe base64 into `bash` blindly. Decode first (`| base64 -d`) and read it.
- To encode a command without a trailing newline: `echo -n 'cmd' | base64`.
- `sudo echo ... | bash` is pointless: `sudo` only applies to `echo`, not to `bash`.
