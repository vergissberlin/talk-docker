# 💬 Talk: Docker pour les débutants

```text
Dauer:              1h 30min - 2h
Niveau:             Einsteiger (keine Vorkenntnisse notwendig)
Zielgruppe:         Einsteiger, die Docker kennenlernen wollen
Voraussetzungen:    Laptop mit Internetzugang und Docker installiert
Sprache:            Deutsch
Author:             André Lademann <vergissberlin@gmail.com>
```

## Ziel des Talks

À la fin du talc, vous montrez comment vous êtes fondamentalement manipulé avec Docker. Vous pouvez emballer des applications dans Docker Images et les exécuter, les déboguer et les gérer. Si tout se passe bien, vous pouvez à la fin, vous pouvez télécharger votre propre image Docker sur Docker Hub et la partager avec les autres.

* * *

## Termes de l'univers Docker

### Docker

Docker est un logiciel qui permet à des applications d'être effectuées dans des conteneurs. Ces conteneurs sont légers et contiennent tout ce dont vous avez besoin. Ils peuvent être effectués sur tout système d'exploitation sur lequel Docker est installé.

#### Image docker

Une image Docker est un modèle utilisé pour créer des conteneurs. Une image contient tout un[Récipient](#docker-container)Besoin de courir. Une image peut être composée d'une ou plusieurs couches. Chaque couche contient un certain nombre d'instructions qui sont effectuées lors de la création d'un conteneur. Si une image contient plusieurs couches, la dernière couche est utilisée comme base et les couches précédentes sont ajoutées sous forme de superposition.

#### Récipient Docker

Un conteneur est une instance exécutable d'un[Images docker](#docker-image). Un conteneur est un environnement isolé qui se compose d'une série de couches. Chaque couche contient un certain nombre d'instructions qui sont effectuées lors de la création d'un conteneur. Si un conteneur contient plusieurs couches, la dernière couche est utilisée comme base et les couches précédentes sont ajoutées comme superposition.

#### Dockerfile

Un fichier docker est un fichier qui contient des instructions que Docker utilise dans un[Image docker](#docker-image)pour créer. Un fichier docker contient un certain nombre d'instructions conçues lors de la création d'une image. Si un fichier docker contient plusieurs instructions, la dernière instruction est utilisée comme base et les instructions précédentes sont ajoutées sous forme de superposition.

##### Exemple de fichier docker

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Docker Compose

Docker Compose est un outil avec lequel vous pouvez utiliser des environnements d'application avec plusieurs[Conteneurs docker](#docker-container)Basé sur des définitions définies dans un fichier YAML. Il utilise des définitions de service pour construire des environnements entièrement personnalisables avec plusieurs conteneurs qui[Réseaux](#docker-netzwerke)et[Volumes de données](#docker-volumes)peut partager.

#### Exemple Docker Compose

```yaml
version: '3.7'

services:
  web:
    image: nginx:latest
    ports:
      - "8000:80"
    volumes:
      - .:/code
    depends_on:
      - redis
  redis:
    image: "redis:alpine"
```

### Réseaux de docker

Les réseaux Docker sont un moyen de connecter les conteneurs. Docker propose trois types de réseaux différents:`bridge`,`host`et`none`.`bridge`est le mode standard dans lequel le conteneur Docker se connecte.`host`Utilisez le réseau hôte de l'hôte sur lequel le conteneur est exécuté.`none`Désactive le réseau du conteneur.

#### Exemple de docker composé avec le réseau

```yaml
version: '3.1'

services:
  web:
    image: nginx:latest
    ports:
      - "8000:80"
    volumes:
      - .:/code
    depends_on:
      - redis
  redis:
    image: "redis:alpine"
    networks:
      - backend

networks:
    backend:
```

### Volumes de docker

Les volumes de Docker sont un moyen de partager des données entre les conteneurs. Docker propose deux types de volumes différents:`bind`et`volume`.`bind`Loue un répertoire sur l'hôte à un répertoire dans le conteneur.`volume`Crée un volume géré par Docker.

#### Exemple de docker composé avec le volume

```yaml
version: '3.1'

services:
  web:
    image: nginx:latest
    ports:
      - "8000:80"
    volumes:
      - .:/code
    depends_on:
      - redis
  redis:
    image: "redis:alpine"
    volumes:
      - redis-data:/data

volumes:
    redis-data:
```

* * *

## Atelier

### Préparation

Vérifiez les choses suivantes à préparer:

-   [ ] Docker installé et exécuté -up`docker version`
-   [ ] Docker compose installé et exécuté -up`docker compose version`
-   [ ] Vous êtes à[Hub docker](https://hub.docker.com/)inscrit`docker login`
-   [ ] **Facultatif:**Appuyez pour ce dépôt à l'étoile ⭐ dans le coin supérieur droit: D
-   [ ] Fork (E) Ces référentiels sur GitHub et Clone (E) sur votre ordinateur

### Tâches

**Le but des tâches**Est-ce un[Image docker](#docker-image)Pour créer qui réalise une application Web simple. L'application Web doit sortir une page HTML simple. Ensuite, nous commencerons l'image Docker localement et verrons comment nous pouvons appeler l'application via le navigateur.

1.  Création d'un fichier docker
2.  Début d'un conteneur
3.  Logging des Containers mit`docker logs`
4.  Appelez un site Web dans le navigateur
5.  Debugging des Containers mit`docker exec`
6.  Neustart des Containers
7.  Stoppen des Containers
8.  Utiliser des supports de volume
9.  Publiez votre propre image dans le registre
10. Création d'un fichier docker compose
11. Démarrer, arrêter et déboguer une demande avec Docker Compose
12. **Prime:**Établissement d'une action github qui construit automatiquement l'image et dans le registre push

### Tâche 1 - Création d'un fichier docker

Tout d'abord, nous voulons regarder une image Docker simple. Pour ce faire, nous créons un répertoire et créons un fichier appelé`Dockerfile`Avec le contenu suivant:

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Tâche 2 - Début d'un conteneur

Maintenant, nous pouvons commencer à construire l'image. Pour ce faire, nous passons au répertoire et réalisons la commande suivante:

```bash
docker build -t workshop .
```

Le`-t`Le drapeau donne un nom à l'image. Le point à la fin de la commande indique que le fichier docker réside dans le répertoire actuel.

Maintenant, nous pouvons démarrer le conteneur:

```bash
docker run workshop
```

### Tâche 3 - journalisation du conteneur avec`docker logs`

Nous pouvons utiliser le journal du conteneur avec la commande`docker logs`afficher. Pour ce faire, nous avons besoin de l'ID de conteneur, que nous avec la commande`docker ps`peut afficher.

```bash
docker logs <container-id>
```

### Tâche 4 - Appel d'un site Web dans le navigateur

Nous avons besoin d'un serveur Web pour un site Web. Pour ce faire, nous créons un nouveau fichier appelé`Dockerfile`Avec le contenu suivant:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

Pour que nous puissions accéder au site Web dans le navigateur, nous créons un nouveau fichier appelé`index.html`Avec le contenu suivant:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Workshop</title>
  </head>
  <body>
    <h1>Workshop</h1>
  </body>
</html>
```

Maintenant, nous pouvons construire l'image et démarrer le conteneur:

```bash
docker build -t workshop .
docker run -p -d 8000:80 workshop
```

Le`-p`L'indicateur lie le port 80 du conteneur au port 8000 de l'hôte. Le`-d`Le drapeau démarre le conteneur en arrière-plan.

### Exercice 5 - débogage du conteneur avec`docker exec`

Nous pouvons utiliser le contenu du conteneur avec la commande`docker exec`afficher. Pour ce faire, nous avons besoin de l'ID de conteneur, que nous avec la commande`docker ps`peut afficher.

```bash
docker exec -it <container-id> bash
```

### Tâche 6 - Redémarrer du conteneur

Nous pouvons utiliser le conteneur avec la commande`docker restart`recommencer. Pour ce faire, nous avons besoin de l'ID de conteneur, que nous avec la commande`docker ps`peut afficher.

```bash
docker restart <container-id>
```

### Tâche 7 - Arrêtez le conteneur

Nous pouvons utiliser le conteneur avec la commande`docker stop`arrêt. Pour ce faire, nous avons besoin de l'ID de conteneur, que nous avec la commande`docker ps`peut afficher.

```bash
docker stop <container-id>
```

### Tâche 8 - Utilisation des supports de volume

Nous pouvons utiliser le conteneur avec la commande`docker run`avec`-v`Démarrer le drapeau. Pour ce faire, nous avons besoin du chemin vers le répertoire de l'hôte et du chemin dans le conteneur.

```bash
docker run -v $PWD:/var/www/html workshop
```

Variable de mort`$PWD`Spécifie le chemin d'accès au répertoire actuel.
Selon le côlon, le chemin est dans le conteneur.

### Tâche 9 - Publiez votre propre image dans le registre

Nous pouvons utiliser notre propre image avec la commande`docker push`Téléchargez vers le registre. Pour ce faire, nous avons besoin du nom de l'image et du jour.

```bash
docker push <image-name>:<tag>
```

La journée est facultative. S'il n'est pas spécifié,`latest`utilisé.

### Tâche 10 - Création d'un fichier docker compose

Nous pouvons avoir plusieurs conteneurs avec la commande`docker run`commencer. Pour ce faire, nous avons besoin des noms des images et des ports.

```bash
docker run -p 8000:80 workshop
docker run -p 8001:80 workshop
```

Nous pouvons également créer un fichier Docker Compose. Pour ce faire, nous créons un nouveau fichier appelé`docker-compose.yml`Avec le contenu suivant:

```yaml
version: '3.7'
services:
  workshop:
    image: workshop
    ports:
      - 8000:80
  workshop2:
    image: workshop
    ports:
      - 8001:80
```

Vous pouvez trouver un large matériel sur le sujet[ici](Material/Task-Task-10/task-10.md).

### Tâche 11 - Démarrez une application avec un Docker Compose

Nous pouvons utiliser la commande`docker-compose up` starten.

```bash
docker-compose up -d
```

Le`-d`Flag démarre l'application comme Docker en arrière-plan.

**Voici une liste des commandes les plus importantes:**

```bash
docker-compose up -d # Startet die Anwendung im Hintergrund
docker-compose down # Stoppt die Anwendung
docker-compose ps # Zeigt die laufenden Container an
docker-compose logs # Zeigt die Logs der Container an
docker-compose exec <service> bash # Startet eine Shell im Container
```

### Tâche 12 - Configuration d'une action github

Nous pouvons créer une action github qui se trouve sur le`main`Branche une nouvelle image dans le registre Hochlädt. Pour ce faire, nous créons un nouveau fichier appelé`.github/workflows/docker.yml`Avec le contenu suivant:

```yaml
name: Docker
on:
  push:
    branches:
      - main
jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Build and push
            uses: docker/build-push-action@v2
            with:
            push: true
            tags: workshop
```

L'action github est sur le`main` Branch ausgeführt. Sie baut das Image und lädt es in die Registry hoch.

Vous pouvez trouver plus d'informations sur les actions GitHub[ici](Material/Task-Task-Bonus/task-bonus.md).

* * *

## À côté de

Dans cette conférence, nous avons traité les bases de Docker. Nous avons vu comment Docker est utilisé pour créer et gérer des conteneurs.

Que pouvez-vous faire pour la conversation pour que quelque chose soit coincé?

1.  Créer des images Docker pour un projet actuel
2.  Créer un fichier Docker Compose
3.  Pour Streber: Jetez un œil à Docker Swarm et Kubernetes

## Contribuer

Avez-vous des suggestions d'amélioration? Puis aime créer une demande de traction ou écrire quelques lignes dans[Forum pour la discussion](https://github.com/vergissberlin/talk-docker/discussions)ou[Gazouillement](https://twitter.com/vergissberlin).
