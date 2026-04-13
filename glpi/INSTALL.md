# GLPI 11.0.2 — Procédure d'installation

---

## A. Prérequis

1. VM Debian Trixie configurée — réseau fonctionnel
2. Effectuer un **snapshot** avant de commencer
3. Se connecter en SSH pour faciliter les copier/coller

```bash
# Passer en root
su -

# Vérifier la version Debian
cat /etc/debian_version

# Mettre à jour le système
apt update && apt upgrade -y
```

---

## B. Installation Apache, MariaDB et PHP

```bash
# Stack web principale
apt install apache2 mariadb-server php -y

# Gestionnaire PHP-FPM
apt install php-fpm -y

# Dépendances PHP requises par GLPI
apt install php-{mysql,mbstring,curl,gd,xml,intl,ldap,apcu,xmlrpc,zip,bz2,bcmath} -y
```

---

## C. Configuration de la base de données MariaDB

```bash
mariadb
```

```sql
-- Création de la base de données
CREATE DATABASE glpi_bdd;

-- Création de l'utilisateur avec droits sur la base
GRANT ALL PRIVILEGES ON glpi_bdd.* TO admin_glpi@localhost IDENTIFIED BY "VotreMotDePasse";

EXIT;
```

---

## D. Téléchargement et déploiement de GLPI

```bash
# Téléchargement de GLPI 11.0.2
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/11.0.2/glpi-11.0.2.tgz

# Décompression dans /var/www/html
tar -xvzf glpi-11.0.2.tgz -C /var/www/html

# Droits propriétaires à www-data
chown -R www-data /var/www/html
```

---

## E. Configuration du VirtualHost Apache

```bash
# Vérifier la version PHP installée
php -v
```

![Version PHP](./screenshots/01-php-version.png)

```bash
# Créer le VirtualHost GLPI
nano /etc/apache2/sites-available/glpi.conf
```

```apache
<VirtualHost *:80>
    ServerName $HOST
    ServerAlias $ADRESSE_IP_SERVEUR
    DocumentRoot /var/www/html

    Alias /glpi /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Options -Indexes +FollowSymLinks
        Require all granted

        RewriteEngine On
        RewriteBase /glpi/
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule ^ index.php [QSA,L]
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.4-fpm.sock|fcgi://localhost"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/glpi_error.log
    CustomLog ${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
```

> Adapter `ServerAlias` avec l'IP de votre serveur et la version PHP relevée à l'étape précédente.

---

## F. Organisation des dossiers — recommandation FHS (facultatif)

Structure recommandée par la documentation GLPI pour séparer config, données et logs.

```bash
# Dossier de configuration
mkdir /etc/glpi
mv /var/www/html/glpi/config /etc/glpi
chown -R www-data /etc/glpi

# Dossier des données
mv /var/www/html/glpi/files /var/lib/glpi

# Dossier des logs
mkdir /var/log/glpi
chown www-data /var/log/glpi

# Dossier des plugins
mkdir /var/lib/glpi/plugins
mv /var/www/html/glpi/marketplace /var/lib/glpi/plugins
mv /var/www/html/glpi/plugins /var/lib/glpi/plugins
chown www-data /var/lib/glpi/plugins
```

### Déclaration des chemins dans GLPI

```bash
# Fichier local_define.php
cd /etc/glpi
nano local_define.php
```

```php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi');
define('GLPI_LOG_DIR', '/var/log/glpi');
define('GLPI_MARKETPLACE_DIR', '/var/lib/glpi/plugins');
```

```bash
# Fichier downstream.php
cd /var/www/html/glpi/inc
nano downstream.php
```

```php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```

---

## G. Sécurisation des sessions PHP

```bash
nano /etc/php/8.4/apache2/php.ini
# Idem pour :
nano /etc/php/8.4/cli/php.ini
```

Rechercher et modifier :

```ini
; Empêcher JavaScript d'accéder aux cookies
session.cookie_httponly = On

; Limiter l'envoi de cookies cross-origin
session.cookie_samesite = Lax

; Uniquement si certificat SSL/TLS configuré
; session.cookie_secure = On
```

![Configuration php.ini](./screenshots/02-php-ini.png)

---

## H. Activation des modules Apache

```bash
# PHP-FPM
a2enmod proxy_fcgi setenvif

# URL dynamiques et redirections
a2enmod rewrite

# Délégation PHP
a2enconf php*-fpm

# Désactiver le site par défaut
a2dissite 000-default.conf

# Activer le site GLPI
a2ensite glpi.conf

# Redémarrer Apache
systemctl restart apache2
```

---

## I. Installation via l'assistant web

Ouvrir le navigateur sur la machine cliente :

```
http://IP_SERVEUR/glpi
```

![Page d'accueil GLPI](./screenshots/03-glpi-accueil.png)

Suivre les étapes de l'assistant :

**Étape 0** — Vérification de la compatibilité

![Étape 0 — Vérification](./screenshots/04-glpi-etape0.png)

**Étape 1** — Configuration de la connexion à la base de données

```
Serveur SQL : localhost
Utilisateur SQL : admin_glpi
Mot de passe SQL : <votre mot de passe>
```

![Étape 1 — Base de données](./screenshots/05-glpi-etape1-db.png)

**Étape 2** — Sélection de la base de données existante `glpi_bdd`

![Étape 2 — Sélection BDD](./screenshots/06-glpi-etape2-bdd.png)

**Étape 3** — Initialisation de la base de données

![Étape 3 — Initialisation](./screenshots/07-glpi-etape3-init.png)

**Étape 4** — Télémétrie — décocher si non souhaité

**Étape 5** — Informations GLPI Network

**Étape 6** — Installation terminée

![Étape 6 — Installation terminée](./screenshots/08-glpi-etape6-fin.png)

---

## J. Première connexion

```
http://IP_SERVEUR/glpi
Identifiant : glpi
Mot de passe : glpi
```

![Dashboard GLPI](./screenshots/09-glpi-dashboard.png)

> Changer immédiatement les mots de passe par défaut de tous les comptes.

---

## K. Nettoyage post-installation

```bash
# Supprimer l'archive de téléchargement
cd /tmp
rm glpi-11.0.2.tgz

# Supprimer la page par défaut Apache
rm /var/www/html/index.html

# Supprimer le script d'installation — important pour la sécurité
rm /var/www/html/glpi/install/install.php
```

---

## L. Configuration LDAP — Authentification Active Directory

Intégration de GLPI avec le domaine `CreativeFusion-Studios.eu` pour centraliser l'authentification.

### Prérequis

- Serveur AD fonctionnel — `PAR-MERLIN-001`
- Compte de service dédié dans l'AD avec droits de lecture sur l'annuaire
- Connectivité réseau entre le serveur GLPI et le serveur AD sur le port 389 (LDAP) ou 636 (LDAPS)

### Accès à la configuration

```
Configuration → Authentification → Répertoires LDAP → + Ajouter
```

### Paramètres de connexion

| Champ | Valeur |
|---|---|
| Nom | `CreativeFusion-AD` |
| Serveur par défaut | Oui |
| Actif | Oui |
| Serveur | `IP_SERVEUR_AD` ou `PAR-MERLIN-001.CreativeFusion-Studios.eu` |
| Port | `389` |
| Champ identifiant | `samaccountname` |
| Champ de synchronisation | `objectGUID` |

### Filtre de connexion Active Directory

Filtre recommandé — retourne uniquement les utilisateurs actifs (exclut les comptes désactivés et les comptes machines) :

```
(&(objectClass=user)(objectCategory=person)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
```

### BaseDN

```
DC=CreativeFusion-Studios,DC=eu
```

> Attention — écrire sans espaces après les virgules. La casse est importante.

### Compte de service

```
DN du compte  : CN=svc_glpi,OU=Compte_services,DC=CreativeFusion-Studios,DC=eu
Mot de passe  : <mot de passe du compte de service>
```

### Préconfiguration Active Directory

Cliquer sur le lien **Préconfiguration → Active Directory** pour charger automatiquement les valeurs par défaut adaptées à AD — cela pré-remplit les champs `objectClass`, `objectCategory` et `samaccountname`.

![Configuration LDAP GLPI](./screenshots/10-glpi-ldap-config.png)

### Test de connexion

Après avoir sauvegardé, cliquer sur **Tester** pour vérifier la connexion au serveur AD.

![Test connexion LDAP](./screenshots/11-glpi-ldap-test.png)

### Synchronisation des utilisateurs

```
Configuration → Authentification → Répertoires LDAP → Importer des utilisateurs
```

Les utilisateurs AD peuvent ensuite se connecter à GLPI avec leurs identifiants de domaine.

![Import utilisateurs AD](./screenshots/12-glpi-ldap-import.png)

### Configuration LDAPS (optionnel — si certificat SSL)

Pour sécuriser la connexion LDAP via TLS :

| Champ | Valeur |
|---|---|
| Serveur | `ldaps://PAR-MERLIN-001.CreativeFusion-Studios.eu` |
| Port | `636` |
| Informations avancées → Utiliser TLS | Oui |

---

## Sources

- [Documentation GLPI Install](https://glpi-install.readthedocs.io/fr/latest/prerequisites.html)
- [GitHub GLPI Project](https://github.com/glpi-project/glpi)

---

> Testé sur Debian Trixie 13.x — PHP 8.4 — MariaDB  
> Dernière mise à jour : avril 2026

> Testé sur Debian Trixie 13.x — PHP 8.4 — MariaDB  
> Dernière mise à jour : avril 2026
