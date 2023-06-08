# infra

# serveur web

## Creation du serveur avec NGINX


## Deploiment des application avec Docker

### conteneurisation de l’application
Assurez-vous que Docker est installé sur votre système. Vous pouvez vérifier cela en exécutant la commande docker version.

Créez un fichier Dockerfile à la racine de votre application. Le Dockerfile est un fichier texte qui contient les instructions pour construire l'image Docker. Voici un exemple de Dockerfile simple pour une application Node.js :

`
#### Utilisez une image de base appropriée
FROM node:14

#### Définissez le répertoire de travail dans le conteneur
WORKDIR /app

#### Copiez les fichiers de l'application dans le conteneur
COPY package.json package-lock.json ./
RUN npm install

#### Copiez le reste des fichiers de l'application dans le conteneur
COPY . .

#### Démarrez l'application lorsque le conteneur démarre
CMD ["npm", "start"]
`
Ouvrez une ligne de commande dans le répertoire où se trouve le Dockerfile.

Construisez l'image Docker en exécutant la commande suivante :


`
docker build -t nom_image .
Remplacez "nom_image" par le nom que vous souhaitez donner à votre image. Le point à la fin de la commande spécifie que le Dockerfile se trouve dans le répertoire actuel.
`

Une fois la construction de l'image terminée, vous pouvez exécuter un conteneur basé sur cette image en utilisant la commande suivante :

`
docker run -d nom_image
Cela démarre un conteneur en arrière-plan (-d) à partir de l'image spécifiée.
`
Pour vérifier que votre conteneur est en cours d'exécution, vous pouvez utiliser la commande `docker ps`. Cela affichera la liste des conteneurs en cours d'exécution.
