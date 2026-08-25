# Homelab VLAN Segmentation

> My home network used to be a flat LAN — everything on the same subnet, every device able to reach every other device. This is the segmentation design I built to fix that.

**Author:** [Chas Carden](https://chascarden.com) · **Guide:** [Read the full writeup](https://chascarden.com/network-segmentation.html)

---

## Why this exists

A flat home network means every device — your smart TV, your NAS, your phone, a compromised IoT plug — can reach every other device. This repo documents the segmentation design I use to fix that: four isolated VLANs, locked by default, with explicit rules for what can talk to what and in which direction.

## The four zones

| Zone | Purpose | Isolation level |
|------|---------|----------------|
| **Main** | Daily-driver laptop and phone | Trusted — can reach Services and NAS |
| **Services** | Self-hosted apps, reverse proxy, Wazuh SIEM | Reachable from Main only |
| **NAS-Backup** | Storage, backups, media | Reachable from Main and Services (backups) |
| **Lab** | Kali, vulnerable targets, pentest machines | Fully isolated — one outbound exception only |

## The one deliberate hole

The Lab VLAN is sealed in and out — **except** for a single outbound log-shipping flow to Wazuh in the Services VLAN. No other cross-zone traffic is permitted from Lab.

This is intentional: complete isolation would contain attacks but leave me blind to them. Allowing only logs out means I can watch every attack land, generate alerts, and practice the full detect-and-respond loop — without exposing anything valuable to the lab machines.

**Isolate, but observe.**

## What's in this repo

- [ ] UniFi VLAN configuration export
- [ ] Firewall rule set — full inter-VLAN policy matrix
- [ ] Wazuh log-shipping exception config
- [ ] Troubleshooting notes — mDNS/casting across VLANs
- [ ] Network diagram (SVG)

> Configs are being added as the build is documented. Check back or watch the repo.

## Stack

- **Network:** UniFi UCG Max · U6 Pro · USW Lite 8 PoE
- **SIEM:** Wazuh (self-hosted, Services VLAN)
- **Server:** UGREEN DXP4800 Plus · TrueNAS SCALE

## Related

- [Full guide on chascarden.com](https://chascarden.com/network-segmentation.html)
- [github.com/ccard-tech](https://github.com/ccard-tech)

---

> ⚠️ This is a personal home lab environment. No credentials, keys, or sensitive network details are committed to this repo.
