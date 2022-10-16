# Workshop - Von Docker bis Kubertes

## Docker

1. Erstellung eines Benutzerkontos auf [Docker Hub](https://hub.docker.com/)
2. Installation von [Docker Desktop](https://www.docker.com/products/docker-desktop) oder Rancher Desktop
3. Starten von Docker Desktop
4. Erstellen eines Docker Containers mit dem [Hello World Image](https://hub.docker.com/_/hello-world)
5. Starte einen weiteren Container mit dem [NGINX Image](https://hub.docker.com/_/nginx)
6. Starte ein Ubuntu und wechsele in den Container mit `docker run -it ubuntu bash`

### Docker Handson

Das Ziel ist es ein Docker image zu erstellen welches eine einfache Webanwendung ausführt. Die Webanwendung soll eine einfache HTML Seite ausgeben.

Anschließend werden wir das docker image lokal starten und uns anschauen wie wir die Anwendung über den Browser aufrufen können.

1. Forken des [Docker Repositories](https://gitbub.com/vergissberlin/devopenspace-dockerkube)
2. Erstellen eines Dockerfiles
3. Starten eines Containers
4. Logging des Containers mit `docker logs`
5. Aufrufen der Webseite im Browser
6. Debugging des Containers mit `docker exec`
7. Installieren von `curl` im Container
8. Neustart des Containers
9. Stoppen des Containers
10. Aufzeigen wie man das Image in einer Docker Registry hochladen kann
11. Anregen das Image in einer Build Pipeline erstellen und veröffentlichen zu lassen

#### GitHub Actions zur Erstellung von Docker Images

1. Erstellen eines GitHub Accouts
2. Push des Repositories auf GitHub
3. Erstellen eines GitHub Actions Workflows. Siehe [Docker Workflow](https://github.com/marketplace/actions/build-and-push-docker-images)

##### GitHub Actions Workflow Beispiel

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - master  
    pull_request:
        branches:
            - master

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
            uses: actions/checkout@v2
        - name: Build and Push Docker Image
            uses: docker/build-push-action@v2
            with:
            context: .
            push: true
            tags: vergissberlin/devopenspace-dockerkube:latest
            cache-from: type=registry,ref=vergissberlin/devopenspace-dockerkube:latest
            cache-to: type=inline
```

## Docker Compose

1. Erstellen einer docker-compose.yml
    1. Versionsnummer festlegen
    2. Service definieren
       1. Webserver
       2. Datenbank
       3. Redis
    3. Netzwerk definieren
 2. Starten der Container mit `docker-compose up`
 3. Logging der Container mit `docker-compose logs` 
 4. Debuggen der Container mit `docker-compose exec`


