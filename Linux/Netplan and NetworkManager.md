---
tags: [linux, networking, netplan, concept]
date: 2026-10-07
---

# Netplan and NetworkManager

- **Netplan** is Ubuntu's network configuration layer (YAML in `/etc/netplan/`).
- It uses a **renderer**: `networkd` (common on servers) or `NetworkManager` (common on desktops).
- With NetworkManager integration, profiles made via `nmcli` are saved as
  `/etc/netplan/90-NM-<UUID>.yaml`, and a runtime copy is generated in `/run`.
- Apply YAML changes with `sudo netplan apply`.

Used in: [[Lab - Static IP with nmcli]], [[NetworkManager Connection Storage with Netplan]]
