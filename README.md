# T-NSA-810 - Infrastructure Hybride Proxmox

**Module** : T-NSA-810 · **École** : EPITECH · **Formateur** : Sébastien DATTICHES (We Are Cyber)

## Architecture

Infrastructure hybride 2 sites, 3 VMs par site, interconnexion IPsec IKEv2.

| Site | Réseau | Services |
|------|--------|----------|
| Site 1 - On-Prem | 192.168.10.0/24 | pfSense · Grafana + Prometheus + Loki · Vault + NetBox + Nginx |
| Site 2 - Distant | 192.168.20.0/24 | pfSense · Bastion Host · DNS (Unbound) + NTP (Chrony) |

## Stack Technologique

| Catégorie | Technologie | Version |
|-----------|-------------|---------|
| Hyperviseur | Proxmox VE | 9.1.4 |
| OS | Ubuntu Server | 24.04.4 LTS |
| Firewall | pfSense CE | 2.8.x |
| VPN S2S | IPsec IKEv2 | strongSwan |
| VPN Client | OpenVPN | 2.x |
| IaC | Ansible | core 2.17.14 |
| Secrets | HashiCorp Vault | 2.0.3 |
| IPAM | NetBox Community | 4.6.3 |
| Métriques | Prometheus + Node Exporter | 2.51.0 / 1.7.0 |
| Logs | Loki + Promtail | 3.2.0 |
| Dashboard | Grafana | 11.x |
| DNS | Unbound | paquet Ubuntu |
| NTP | Chrony | 4.5 |
| Web interne | Nginx | 1.24.0 |

## Structure du dépôt
ansible/        → Playbooks, rôles et inventaire Ansible (IaC)
architectures/  → Schémas HLD v1_v2.0 et BLD v.1
configs/        → Configurations des services (pfSense, OpenVPN, Vault, NetBox)
docs/           → Documentation complète (PDF_Projet, choix technos, script démo)
scripts/        → Scripts utilitaires

## Utilisation Ansible

### Prérequis

```bash
pip install ansible hvac
ansible-galaxy collection install community.hashi_vault
```

### Configuration des secrets

```bash
cp ansible/group_vars/all.yml.example ansible/group_vars/all.yml
# Éditer all.yml avec les vraies valeurs (ne jamais pousser sur Git)
```

### Commandes

```bash
# Tester la connectivité sur les 4 VMs
ansible all -i ansible/inventory.ini -m ping

# Déployer Node Exporter (4 VMs — idempotent)
ansible-playbook -i ansible/inventory.ini ansible/playbooks/playbook-node-exporter.yml --ask-become-pass

# Déployer DNS + NTP sur Site 2
ansible-playbook -i ansible/inventory.ini ansible/playbooks/playbook-dns-ntp.yml --ask-become-pass

# Démonstration intégration Vault
ansible-playbook -i ansible/inventory.ini ansible/playbooks/playbook-vault-demo.yml --ask-become-pass
```

## Documentation

Voir le dossier `docs/` :
- `HLD_T-NSA-810_v2.html` - vue fonctionnelle globale
- `BLD_T-NSA-810_v3.html` - configuration technique détaillée
- `Liste_Choix_Technologiques_v1.html` - justifications des choix
- `Script_Demonstration_Keynote.html` - script de démo live

## Blocage technique documenté

Le tunnel IPsec est établi (gateway-to-gateway fonctionnel), mais l'accès direct aux hôtes du LAN Site 2 depuis le VPN client ou le LAN Site 1 reste bloqué (cause profonde non identifiée malgré un diagnostic approfondi). **Contournement** : NAT Port Forward dédié sur l'IP WAN pfSense S2 (SSH:2222, Metrics:9101/9102) et ProxyJump Ansible via le Bastion.

## Sécurité

- Aucun secret en clair dans ce dépôt
- `ansible/group_vars/all.yml` exclu par `.gitignore`
- Clés SSH et profils VPN non versionnés
- Voir `.gitignore` pour la liste complète des exclusions
