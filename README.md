# Guides d'installation

> Documentation d'installation pas-à-pas pour des services couramment déployés en environnement professionnel.

![Status](https://img.shields.io/badge/Status-En_cours-orange?style=flat-square)
![Debian](https://img.shields.io/badge/Debian-Trixie_13-A81D33?style=flat-square)
![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square)

---

## Objectif

Ce repo centralise les procédures d'installation que j'ai réalisées et documentées dans mes labs GNS3.  
Chaque guide est structuré de la même façon : contexte, prérequis, installation pas-à-pas, vérification.

---

## Guides disponibles

| Dossier | Service | OS | Statut |
|---|---|---|---|
| [`zabbix/`](./zabbix/) | Supervision Zabbix 7.4 — pile LNPP + TimescaleDB | Debian Trixie 13 | ✅ Disponible |
| [`fogproject/`](./fogproject/) | Déploiement PXE FOGProject — capture et masterisation | Debian Trixie 13 | ✅ Disponible |
| [`glpi/`](./glpi/) | Helpdesk GLPI 11.0.2 + authentification AD | Debian Trixie 13 | 🔄 En cours |
| [`debian-base/`](./debian-base/) | Configuration de base Debian — SSH, sudo, sécurisation | Debian Trixie 13 | 🔄 En cours |
| [`powerdns/`](./powerdns/) | Serveur DNS PowerDNS | Debian Trixie 13 | ⏳ À venir |
| [`kea-dhcp/`](./kea-dhcp/) | Serveur DHCP Kea | Debian Trixie 13 | ⏳ À venir |
| [`openvpn/`](./openvpn/) | Serveur OpenVPN + SSH sécurisé | Debian Trixie 13 | ⏳ À venir |
| [`wazuh/`](./wazuh/) | SIEM Wazuh — serveur + agents | Debian Trixie 13 | ⏳ À venir |

---

## Structure type de chaque guide

```
install-guides/<service>/
├── README.md        ← configuration et environnement
├── INSTALL.md       ← procédure complète pas-à-pas                                    
└── screenshots/     ← captures de validation
```

---

## Environnement de test

Tous les guides sont testés dans un environnement virtualisé :

| Composant | Détail |
|---|---|
| Simulation réseau | GNS3 2.x |
| Hyperviseurs | VirtualBox · VMware Fusion |
| OS principal | Debian Trixie 13.x |
| Accès | SSH par clé  |

---

## Repos associés

- [`lab-gns3-security`](https://github.com/cedric-ribier/lab-gns3-security) — Lab réseau & sécurité
- [`lab-gns3-TSSR`](https://github.com/cedric-ribier/lab-gns3-TSSR) — Infrastructure TSSR complète
- [`Powershell-scripts`](https://github.com/cedric-ribier/Powershell-scripts) — Scripts d'automatisation

---

> Guides rédigés à partir de réalisations concrètes en lab — pas de théorie sans pratique.  
> Dernière mise à jour : avril 2026
