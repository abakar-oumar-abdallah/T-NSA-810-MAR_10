# NetBox — Configuration IPAM

## Accès
URL : http://192.168.10.103 (accessible LAN/VPN uniquement)

## Structure documentée

### Sites
| Nom | Description |
|-----|-------------|
| Site 1 - On-Prem | Proxmox VE Site 1 |
| Site 2 - Distant | Proxmox VE Site 2 |

### Préfixes
| Préfixe | Site | Description |
|---------|------|-------------|
| 192.168.10.0/24 | Site 1 | LAN Site 1 |
| 192.168.20.0/24 | Site 2 | LAN Site 2 |
| 10.8.0.0/24 | — | Tunnel OpenVPN client-to-site |

### Appareils & IPs
| Device | IP | Interface |
|--------|-----|-----------|
| pfSense-S1 | 192.168.10.1/24 | LAN |
| pfSense-S2 | 192.168.20.1/24 | LAN |
| VM-S1-Grafana | 192.168.10.102/24 | eth0 |
| VM-S1-Vault-NetBox | 192.168.10.103/24 | eth0 |
| VM-S2-Bastion | 192.168.20.10/24 | eth0 |
| VM-S2-DNS-NTP | 192.168.20.20/24 | eth0 |
