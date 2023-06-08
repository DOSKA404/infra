# infra

# serveur web

## Creation du serveur avec NGINX


## Deploiment des application avec Docker

### conteneurisation de l’application

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
