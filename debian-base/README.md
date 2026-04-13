# Configuration de base — Debian Trixie 13

Configuration initiale appliquée à chaque serveur Debian du lab avant tout déploiement de service.  
Couvre la mise à jour du système, sudo, SSH sécurisé par clé et l'installation de `tldr`.

---

## Contenu

| Fichier | Description |
|---|---|
| [`INSTALL.md`](./INSTALL.md) | Procédure complète pas-à-pas |
| [`screenshots/`](./screenshots/) | Captures de validation |

---

## Ce que couvre ce guide

| Étape | Description |
|---|---|
| Mise à jour | `apt update && apt full-upgrade` |
| Sudo | Installation et ajout de l'utilisateur |
| OpenSSH | Installation, vérification, clé ed25519 |
| tldr | Utilitaire de consultation des man pages simplifié |

---

## Pourquoi `tldr` ?

`tldr` propose des exemples concrets et lisibles pour les commandes Linux — là où `man` est exhaustif mais dense. Idéal pour retrouver rapidement la syntaxe d'une commande sans parcourir des pages de documentation.

```bash
# Exemple — au lieu de man tar
tldr tar
```

---

## Prérequis

- VM Debian Trixie 13.x installée
- Accès console ou SSH root

---

> Voir [`INSTALL.md`](./INSTALL.md) pour la procédure complète.  
> Dernière mise à jour : avril 2026
