# Workshop - Von Docker bis Kubernetes

## Docker

Was ist *Docker*? Siehe [README.md](../README.md#Docker)

1. Erstellung eines Benutzerkontos auf [Docker Hub](https://hub.docker.com/)
2. Installation von [Docker Desktop](https://www.docker.com/products/docker-desktop) oder Rancher Desktop
3. Starte von Docker Desktop
4. Log dich auf der Komandozeile mit deinem Benutzerkonto ein `docker login`
5. Starte ein erstes Hello World Image `docker run hello-world`. Mehr Informationen zu diesem Images findest du [Hello World Image](https://hub.docker.com/_/hello-world)
6. Starte einen weiteren Container mit dem [NGINX Image](https://hub.docker.com/_/nginx) auf der Kommandozeile `docker run -d -p 8080:80 --name nginx nginx`. Beachte das nun der Container im Hintergrund läuft und nicht mehr auf der Kommandozeile angezeigt wird und das der Port 8080 auf den Port 80 des Containers gemappt wird.
7. Starte ein Ubuntu und wechsele in den Container mit `docker run -it ubuntu bash`

### Docker hands on

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

#### Dockerfile

Das Dockerfile ist eine Datei welche die Anweisungen für das Erstellen eines Docker Images enthält. Die Anweisungen werden von oben nach unten ausgeführt. Die Anweisungen werden in der Regel in Großbuchstaben geschrieben.

##### Beispiel Dockerfile

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl
CMD ["curl", "http://localhost"]
```

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

Was ist *Docker Compose*? Siehe [README](../README.md#docker-compose)

### Docker Compose hands on

Das Ziel ist es eine Webanwendung mit Docker Compose zu starten. Die Webanwendung soll eine einfache HTML Seite ausgeben. Die Webanwendung soll eine Datenbank und einen Redis Cache verwenden.

Anschließend werden wir die Anwendung über den Browser aufrufen können.

1. Forken des [Docker Compose Repositories](https://gitbub.com/vergissberlin/devopenspace-dockerkube)
2. Erstellen einer docker-compose.yml
3. Starten der Container mit `docker compose up`
4. Logging der Container mit `docker compose logs`
5. Debuggen der Container mit `docker compose exec`


#### docker-compose.yml

Das docker-compose.yml ist eine Datei welche die Anweisungen für das Starten von mehreren Docker Images enthält. Die Anweisungen werden von oben nach unten ausgeführt. Die Anweisungen werden in der Regel in Kleinbuchstaben geschrieben.

##### Beispiel docker-compose.yml

```yaml
version: "3.9"

services:
  web:
    image: vergissberlin/devopenspace-dockerkube:latest
    build: .
    ports:
      - "8080:80"
    depends_on:
      - db
      - redis
    networks:
      - devopenspace
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: password
    networks:
        - devopenspace
  redis:
    image: redis:latest
    networks:
        - devopenspace

networks:
  default:
    external:
      name: devopenspace

```
