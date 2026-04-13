# GLPI 11.0.2 — Guide d'installation

Installation de GLPI 11.0.2 sur Debian Trixie 13 avec Apache, MariaDB et PHP-FPM.  
Configuration conforme FHS et sécurisation des sessions.

---

## Contenu

| Fichier | Description |
|---|---|
| [`INSTALL.md`](./INSTALL.md) | Procédure complète pas-à-pas avec captures |
| [`config/glpi.conf`](./config/glpi.conf) | VirtualHost Apache pour GLPI |
| [`screenshots/`](./screenshots/) | Captures de validation |

---

## Environnement

| Composant | Détail |
|---|---|
| OS | Debian Trixie 13.x |
| Serveur web | Apache2 + PHP-FPM |
| Base de données | MariaDB |
| PHP | 8.4.x |
| Version GLPI | 11.0.2 |
| Accès | SSH |

---

## Prérequis

- VM Debian Trixie 13.x installée et à jour
- Accès SSH ou accès console
- Droits root

---

## Résultat attendu

- Interface GLPI accessible sur `http://IP_SERVEUR/glpi`
- Base de données MariaDB configurée et connectée
- Comptes par défaut créés — à modifier immédiatement

---

## Comptes par défaut à l'installation

| Compte | Identifiants | Rôle |
|---|---|---|
| Administrateur | `glpi` / `glpi` | Super-Admin |
| Technicien | `tech` / `tech` | Technicien |
| Normal | `normal` / `normal` | Utilisateur normal |
| Post-only | `post-only` / `postonly` | Saisie de tickets uniquement |

> **Important** — changer tous ces mots de passe immédiatement après installation.

---

> Voir [`INSTALL.md`](./INSTALL.md) pour la procédure complète.  
> Dernière mise à jour : avril 2026
