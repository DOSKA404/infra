# infra

# serveur web

## Creation du serveur avec NGINX


## Deploiment des application avec Docker



dans cette exemple l'aplication conteunerisé est en golang

Assurez-vous d'avoir Docker installé sur votre machine.

Créez un fichier Dockerfile dans le répertoire racine de votre application Go. Ce fichier Dockerfile définira les instructions pour la construction de l'image Docker.

Ouvrez le fichier Dockerfile dans un éditeur de texte et ajoutez les lignes suivantes :

```
# Utilisez une image de base avec Go préinstallé
FROM golang:latest

# Définissez le répertoire de travail dans le conteneur
WORKDIR /app

# Copiez les fichiers du répertoire local vers le répertoire de travail du conteneur
COPY . .

# Construisez l'application Go
RUN go build -o main .

# Utilisez une image de base Nginx
FROM nginx:latest

# Copiez le binaire de l'application Go construite dans le répertoire approprié de Nginx
COPY --from=0 /app/main /usr/share/nginx/html

# Exposez le port 80 pour le trafic HTTP
EXPOSE 80

# Démarrez Nginx lors du démarrage du conteneur
CMD ["nginx", "-g", "daemon off;"]
```
Enregistrez le fichier Dockerfile.

Ouvrez une fenêtre de terminal et accédez au répertoire contenant votre Dockerfile.

Construisez l'image Docker en exécutant la commande suivante :

```
docker build -t monapp .
```
Cela va créer une nouvelle image Docker nommée "monapp" en utilisant le Dockerfile du répertoire actuel.

Une fois que la construction de l'image est terminée, vous pouvez exécuter un conteneur basé sur cette image en utilisant la commande suivante :
```
docker run -d -p 80:80 monapp
```
Cela va démarrer un nouveau conteneur en arrière-plan et lier le port 80 de votre machine hôte au port 80 du conteneur.

Vous pouvez maintenant accéder à votre application Go via un navigateur en utilisant l'URL http://localhost.
Assurez-vous que votre application Go écoute sur le port approprié (par défaut, le port 80) pour que Nginx puisse la servir correctement.

## Geration du certificat SSL

Générez une clé privée SSL en utilisant la commande openssl :

```
sudo openssl genpkey -algorithm RSA -out private.key
```
Créez une demande de signature de certificat (CSR - Certificate Signing Request) en utilisant la clé privée précédemment générée :

```
sudo openssl req -new -key private.key -out csr.pem
```
Vous serez invité à fournir des informations pour le certificat, telles que le nom commun (domaine) pour lequel vous souhaitez générer le certificat.

Auto-signez le certificat en utilisant le CSR et la clé privée :

```
sudo openssl x509 -req -days 365 -in csr.pem -signkey private.key -out certificate.crt
```
Cela générera un certificat auto-signé valide pour 365 jours.

Déplacez les fichiers clés générés dans les emplacements appropriés pour Nginx :

```
sudo mv private.key /etc/ssl/private/
sudo mv certificate.crt /etc/ssl/certs/
```
Configurez Nginx pour utiliser le certificat SSL auto-signé en éditant le fichier de configuration du site :

```
sudo nano /etc/nginx/sites-available/votre_site.conf
```
À l'intérieur du bloc server, ajoutez ou modifiez les lignes suivantes pour activer SSL et spécifier le chemin vers les clés :

```
listen 443 ssl;
ssl_certificate /etc/ssl/certs/certificate.crt;
ssl_certificate_key /etc/ssl/private/private.key;
```
Sauvegardez et fermez le fichier.

Vérifiez la configuration de Nginx pour vous assurer qu'il n'y a pas d'erreurs :
```
sudo nginx -t
```
Redémarrez le service Nginx pour appliquer les modifications :
```
sudo systemctl restart nginx
```
