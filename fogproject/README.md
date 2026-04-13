# FOGProject — Guide d'installation

Déploiement d'un serveur FOGProject pour la capture et le déploiement d'images système via PXE.  
Testé dans un environnement virtualisé VirtualBox — lab TSSR CreativeFusion Studios.

---

## Contenu

| Fichier | Description |
|---|---|
| [`INSTALL.md`](./INSTALL.md) | Procédure complète pas-à-pas avec captures |
| [`screenshots/`](./screenshots/) | Captures de validation |

---

## Environnement

| Machine | Rôle | Ressources |
|---|---|---|
| pfSense | Routeur / DHCP / PXE | 1 vCPU — 2 Go RAM — 16 Go |
| Debian — Serveur FOG | FOG + TFTP/PXE + NFS | 2 vCPU — 4 Go RAM — 120 Go |
| Debian — Bureau | Administration FOG | 2 vCPU — 4 Go RAM — 40 Go |
| Windows 11 — Modèle | Capture de l'image | 2 vCPU — 4 Go RAM — 80 Go |
| Windows 11 — Cible | Déploiement | 2 vCPU — 4 Go RAM — 80 Go |

**Réseau LAN** : `10.0.80.0/24` — Gateway pfSense `10.0.80.254`  
**Serveur FOG** : `10.0.80.1` — IP fixe obligatoire

---

## Prérequis

- 5 VMs VirtualBox configurées et interconnectées
- pfSense installé et DHCP actif sur le LAN
- VM Debian serveur FOG avec IP fixe
- VM Debian bureau pour accès à l'interface web FOG

---

## Résultat attendu

- Interface FOG accessible sur `http://IP_SERVEUR_FOG/fog/management`
- Image Windows 11 capturée et stockée sur le serveur
- Déploiement automatique sur nouvelle machine via boot PXE

---

> Voir [`INSTALL.md`](./INSTALL.md) pour la procédure complète.  
> Dernière mise à jour : avril 2026
