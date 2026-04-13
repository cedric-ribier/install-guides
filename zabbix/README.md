# Zabbix 7.4 — Guide d'installation

Déploiement de Zabbix 7.4 sur Debian Trixie 13.x avec pile LNPP et TimescaleDB.  
Testé dans un environnement GNS3 virtualisé — lab TSSR CreativeFusion Studios.

---

## Contenu

| Fichier | Description |
|---|---|
| [`INSTALL.md`](./INSTALL.md) | Procédure complète pas-à-pas avec captures |
| [`screenshots/`](./screenshots/) | Captures de validation |

---

## Environnement

| Composant | Détail |
|---|---|
| OS | Debian Trixie 13.x |
| Serveur | `par-deadpool-003` |
| Base de données | PostgreSQL + TimescaleDB |
| Frontend | Nginx + PHP-FPM |
| Accès | SSH par clé ed25519 |

---

## Dimensionnement

| Ressource | Valeur |
|---|---|
| vCPU | 4 |
| RAM | 8 à 12 Go |
| Stockage | 80 à 120 Go SSD |

---

## Prérequis

- VM Debian Trixie 13.x installée et à jour
- Accès SSH configuré
- Droits sudo

---

## Résultat attendu

- Dashboard Zabbix accessible sur `http://IP_SERVEUR/zabbix`
- Agents déployés sur les équipements supervisés
- Métriques collectées — CPU, RAM, réseau, services

---

> Voir [`INSTALL.md`](./INSTALL.md) pour la procédure complète.  
> Dernière mise à jour : avril 2026
