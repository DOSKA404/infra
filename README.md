# infra

# serveur web

## Étape 1 : Prérequis
Assurez-vous d'avoir les éléments suivants installés sur votre machine :

Docker : Suivez les instructions spécifiques à votre système d'exploitation pour installer Docker.
Nginx : Installez Nginx sur votre système en suivant la documentation officielle.
OpenSSL : Installez OpenSSL sur votre système pour la génération des certificats auto-signés.

## Étape 2 : Configuration du serveur web Nginx

Créez un répertoire pour stocker les fichiers de configuration Nginx :

```
sudo mkdir /etc/nginx/sites-available
sudo mkdir /etc/nginx/sites-enabled
```
Ouvrez le fichier de configuration principal de Nginx :

```
sudo nano /etc/nginx/nginx.conf
```
Ajoutez la directive suivante dans la section http :

```
include /etc/nginx/sites-enabled/*;
```
Sauvegardez et fermez le fichier de configuration.

Créez un fichier de configuration pour votre site principal :

```
sudo nano /etc/nginx/sites-available/monsite.conf
```
Ajoutez le bloc de configuration suivant pour votre site principal :

```
server {
    listen 80;
    server_name monsite.com www.monsite.com;

    location / {
        proxy_pass http://localhost:8000;  # Remplacez le port selon les besoins de votre application principale
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
Modifiez monsite.com et www.monsite.com avec votre nom de domaine principal. Remplacez également le port (8000) par le port utilisé par votre application principale.

Sauvegardez et fermez le fichier de configuration.

Créez des liens symboliques pour activer la configuration de votre site principal :

```
sudo ln -s /etc/nginx/sites-available/monsite.conf /etc/nginx/sites-enabled/
```
## Étape 3 : Génération des certificats SSL auto-signés

Générez une clé privée et un certificat auto-signé en utilisant OpenSSL :

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/monsite.key -out /etc/nginx/ssl/monsite.crt
```
Remplacez monsite par le nom de votre site principal.

Répondez aux questions du formulaire de génération du certificat selon vos préférences.

Assurez-vous que les fichiers de clé privée (monsite.key) et de certificat (monsite.crt) sont correctement générés dans le répertoire spécifié.

## Étape 4 : Configuration des sous-domaines

Ouvrez le fichier de configuration de votre site principal :

```
sudo nano /etc/nginx/sites-available/monsite.conf
```
Ajoutez les blocs de configuration suivants pour chaque sous-domaine souhaité :

```
server {
    listen 80;
    server_name sousdomaine1.monsite.com;

    location / {
        proxy_pass http://localhost:8001;  # Remplacez le port selon les besoins de votre application pour sousdomaine1.monsite.com
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

server {
    listen 80;
    server_name sousdomaine2.monsite.com;

    location / {
        proxy_pass http://localhost:8002;  # Remplacez le port selon les besoins de votre application pour sousdomaine2.monsite.com
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
Ajoutez autant de blocs de configuration que nécessaire pour vos sous-domaines, en remplaçant les ports (8001, 8002, etc.) par les ports utilisés par les applications correspondantes.

Sauvegardez et fermez le fichier de configuration.

Créez des liens symboliques pour activer la configuration de vos sous-domaines :

```
sudo ln -s /etc/nginx/sites-available/monsite.conf /etc/nginx/sites-enabled/
```
## Étape 5 : Configuration du HTTPS avec SSL

Ouvrez le fichier de configuration de votre site principal :

```
sudo nano /etc/nginx/sites-available/monsite.conf
```
Modifiez le bloc de configuration pour le port 443 (HTTPS) en ajoutant les directives SSL :

```
server {
    listen 443 ssl;
    server_name monsite.com www.monsite.com;

    ssl_certificate /etc/nginx/ssl/monsite.crt;
    ssl_certificate_key /etc/nginx/ssl/monsite.key;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
Sauvegardez et fermez le fichier de configuration.

## Étape 6 : Redémarrage du serveur Nginx
Redémarrez le serveur Nginx pour appliquer les modifications en utilisant la commande suivante :

shell
Copy code
sudo service nginx restart
Après avoir suivi ces étapes, votre serveur web Nginx sera configuré avec votre site principal, vos sous-domaines, et le support SSL avec des certificats auto-signés. Assurez-vous que les conteneurs Docker correspondants sont en cours d'exécution sur les ports spécifiés.

Vérifiez l'accès à votre site principal et à vos sous-domaines en ouvrant un navigateur Web et en accédant à l'URL correspondante (par exemple, http://monsite.com ou http://sousdomaine1.monsite.com).
