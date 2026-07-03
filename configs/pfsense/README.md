# pfSense — Configuration Site 1 & Site 2

Les configurations pfSense ne peuvent pas être versionnées directement
(format XML propriétaire contenant des secrets).

## Éléments configurés

### pfSense S1 (5.196.51.234)
- **WAN** : 5.196.51.234 — **LAN** : 192.168.10.1
- Firewall : règles WAN whitelist par IP, Killswitch S2 (désactivé par défaut)
- IPsec : IKEv2, Mutual PSK, AES-256, SHA256, DH Group 14
- OpenVPN : Remote Access SSL/TLS + User Auth, UDP:1194, AES-256-GCM
- NAT Outbound : Hybrid (Do not NAT pour trafic IPsec)

### pfSense S2 (5.135.202.78)
- **WAN** : 5.135.202.78 — **LAN** : 192.168.20.1
- NAT Port Forward : WAN:2222→Bastion:22, WAN:9101→Bastion:9100, WAN:9102→DNS-NTP:9100
- NAT Outbound : Hybrid (Do not NAT pour trafic retour IPsec)
