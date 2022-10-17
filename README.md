# 💬 Talk: Docker für Einsteiger

> Ein Einsteiger-Workshop Tutorial für Docker-Neulinge und -Neugierige von [André Lademann](https://blog.andrelademann.de/).
> Dauer:  1h 30min - 2h

## Ziel des Talks

Am Ende des Talks weist Du, wie du wie Du grundlegend mit Docker umgehst. Du kannst Anwendungen in Docker-Images packen und diese ausführen, debuggen und verwalten. Wenn alles gut läuft, kannst Du am Ende dein eigenes Docker Image auf Docker Hub hochladen und mit anderen teilen.

## Vorraussetzungen

- Laptop mit Internetzugang
- Mindestens 1h Zeit
- Bei Windows: WSL2 installiert

---

## Begriffe aus dem Docker-Universum

### Docker

Docker ist eine Software, die es ermöglicht, Anwendungen in Containern auszuführen. Diese Container sind leichtgewichtig und enthalten alles, was sie benötigen, um zu laufen. Sie können auf jedem Betriebssystem ausgeführt werden, auf dem Docker installiert ist.

#### Docker Image

Ein Docker Image ist eine Vorlage, die Docker verwendet, um Container zu erstellen. Ein Image enthält alles, was ein [Container](#docker-container) benötigt, um zu laufen. Ein Image kann aus einem oder mehreren Schichten bestehen. Jede Schicht enthält eine Reihe von Anweisungen, die beim Erstellen eines Containers ausgeführt werden. Wenn ein Image mehrere Schichten enthält, wird die letzte Schicht als Basis verwendet und die vorherigen Schichten als Overlay hinzugefügt.

#### Docker Container

Ein Container ist eine ausführbare Instanz eines [Docker Images](#docker-image). Ein Container ist eine isolierte Umgebung, die aus einer Reihe von Schichten besteht. Jede Schicht enthält eine Reihe von Anweisungen, die beim Erstellen eines Containers ausgeführt werden. Wenn ein Container mehrere Schichten enthält, wird die letzte Schicht als Basis verwendet und die vorherigen Schichten als Overlay hinzugefügt.

#### Dockerfile

Ein Dockerfile ist eine Datei, die Anweisungen enthält, die Docker verwendet, um ein [Docker Image](#docker-image) zu erstellen. Ein Dockerfile enthält eine Reihe von Anweisungen, die beim Erstellen eines Images ausgeführt werden. Wenn ein Dockerfile mehrere Anweisungen enthält, wird die letzte Anweisung als Basis verwendet und die vorherigen Anweisungen als Overlay hinzugefügt.

##### Beispiel Dockerfile

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Docker Compose

Docker Compose ist ein Tool, mit dem Sie Anwendungsumgebungen mit mehreren [Docker Containern](#docker-container) basierend auf in einer YAML-Datei festgelegten Definitionen ausführen können. Er verwendet Dienst-Definitionen zum Aufbau voll anpassbarer Umgebungen mit mehreren Containern, die [Netzwerke](#docker-netzwerke) und [Datenvolumes](#docker-volumes) teilen können.

#### Beispiel Docker Compose

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

### Docker Netzwerke

Docker Netzwerke sind eine Möglichkeit, Container miteinander zu verbinden. Docker bietet drei verschiedene Arten von Netzwerken an: `bridge`, `host` und `none`. `bridge` ist der Standardmodus, in dem Docker Container miteinander verbindet. `host` verwendet das Host-Netzwerk des Hosts, auf dem der Container ausgeführt wird. `none` deaktiviert das Netzwerk für den Container.

#### Beispiel Docker Compose mit Netzwerk

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

### Docker Volumes

Docker Volumes sind eine Möglichkeit, Daten zwischen Containern zu teilen. Docker bietet zwei verschiedene Arten von Volumes an: `bind` und `volume`. `bind` bindet ein Verzeichnis auf dem Host an ein Verzeichnis im Container. `volume` erstellt ein Volume, das von Docker verwaltet wird.

#### Beispiel Docker Compose mit Volume

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

---

## Workshop

### Vorbereitung

Prüfe folgende Dinge zur Vorbeitung:

- [ ] Docker installiert und lauffähig `docker version`
- [ ] Docker Compose installiert und lauffähig `docker compose version`
- [ ] Du bist bei [Docker Hub](https://hub.docker.com/) angemeldet `docker login`
- [ ] Fork(e) dieses Repositories

### Aufgaben

**Das Ziel der Aufgaben** ist es ein [Docker Image](#docker-image) zu erstellen, welches eine einfache Webanwendung ausführt. Die Webanwendung soll eine einfache HTML Seite ausgeben. Anschließend werden wir das docker image lokal starten und uns anschauen wie wir die Anwendung über den Browser aufrufen können.

1. Erstellen eines Dockerfiles
2. Starten eines Containers
3. Logging des Containers mit `docker logs`
4. Aufrufen einer Webseite im Browser
5. Debugging des Containers mit `docker exec`
6. Neustart des Containers
7. Stoppen des Containers
8. Verwenden von Volume Mounts
9. Das eigene Image in der Registry veröffentlichen
10. Erstellen eines Docker Compose Files
11. Starten, Stoppen und Debuggen einer Anwendung mit Docker Compose
12. **Bonus:** Einrichtung einer GitHub Action, die das Image automatisch baut und in die Registry pusht

### Aufgabe 1 - Erstellen eines Dockerfiles

Zunächst wollen wir uns ein einfaches Docker Image anschauen. Dazu erstellen wir ein Verzeichnis und erstellen eine Datei namens `Dockerfile` mit folgendem Inhalt:

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Aufgabe 2 - Starten eines Containers

Nun können wir mit dem Bauen des Images beginnen. Dazu wechseln wir in das Verzeichnis und führen folgenden Befehl aus:

```bash
docker build -t workshop .
```

Das `-t` Flag gibt dem Image einen Namen. Der Punkt am Ende des Befehls gibt an, dass das Dockerfile im aktuellen Verzeichnis liegt.

Nun können wir den Container starten:

```bash
docker run workshop
```

### Aufgabe 3 - Logging des Containers mit `docker logs`

Wir können uns das Log des Containers mit dem Befehl `docker logs` anzeigen lassen. Dazu benötigen wir die Container ID, die wir mit dem Befehl `docker ps` anzeigen lassen können.

```bash
docker logs <container-id>
```

### Aufgabe 4 - Aufrufen einer Webseite im Browser

Für eine Webseite benötigen wir einen Webserver. Dazu erstellen wir eine neue Datei namens `Dockerfile` mit folgendem Inhalt:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

Damit wir die Webseite im Browser aufrufen können, erstellen wir eine neue Datei namens `index.html` mit folgendem Inhalt:

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

Nun können wir das Image bauen und den Container starten:

```bash
docker build -t workshop .
docker run -p -d 8000:80 workshop
```

Das `-p` Flag bindet den Port 80 des Containers an den Port 8000 des Hosts. Das `-d` Flag startet den Container im Hintergrund.

### Aufgabe 5 - Debugging des Containers mit `docker exec`

Wir können uns den Inhalt des Containers mit dem Befehl `docker exec` anzeigen lassen. Dazu benötigen wir die Container ID, die wir mit dem Befehl `docker ps` anzeigen lassen können.

```bash
docker exec -it <container-id> bash
```

### Aufgabe 6 - Neustart des Containers

Wir können den Container mit dem Befehl `docker restart` neustarten. Dazu benötigen wir die Container ID, die wir mit dem Befehl `docker ps` anzeigen lassen können.

```bash
docker restart <container-id>
```

### Aufgabe 7 - Stoppen des Containers

Wir können den Container mit dem Befehl `docker stop` stoppen. Dazu benötigen wir die Container ID, die wir mit dem Befehl `docker ps` anzeigen lassen können.

```bash
docker stop <container-id>
```

### Aufgabe 8 - Verwenden von Volume Mounts

Wir können den Container mit dem Befehl `docker run` mit dem `-v` Flag starten. Dazu benötigen wir den Pfad zum Verzeichnis auf dem Host und den Pfad im Container.

```bash
docker run -v $PWD:/var/www/html workshop
```

Die Variable `$PWD` gibt den Pfad zum aktuellen Verzeichnis an.
Nach dem Doppelpunkt steht der Pfad im Container.

### Aufgabe 9 - Das eigene Image in der Registry veröffentlichen

Wir können das eigene Image mit dem Befehl `docker push` in die Registry hochladen. Dazu benötigen wir den Namen des Images und den Tag.

```bash
docker push <image-name>:<tag>
```

Der Tag ist optional. Wenn er nicht angegeben wird, wird `latest` verwendet.

### Aufgabe 10 - Erstellen eines Docker Compose Files

Wir können mehrere Container mit dem Befehl `docker run` starten. Dazu benötigen wir die Namen der Images und die Ports.

```bash
docker run -p 8000:80 workshop
docker run -p 8001:80 workshop
```

Wir können auch ein Docker Compose File erstellen. Dazu erstellen wir eine neue Datei namens `docker-compose.yml` mit folgendem Inhalt:

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

Weites Material zum Thema findest du [hier](Material/Task-Task-10/task-10.md).

### Aufgabe 11 - Starten einer Anwendung mit Docker Compose

Wir können die Anwendung mit dem Befehl `docker-compose up` starten.

```bash
docker-compose up -d
```

Das `-d` Flag startet die Anwendung wie bei Docker im Hintergrund.

**Hier eine  Liste der wichtigsten Befehle:**

```bash
docker-compose up -d # Startet die Anwendung im Hintergrund
docker-compose down # Stoppt die Anwendung
docker-compose ps # Zeigt die laufenden Container an
docker-compose logs # Zeigt die Logs der Container an
docker-compose exec <service> bash # Startet eine Shell im Container
```

### Aufgabe 12 - Einrichtung einer GitHub Action

Wir können eine GitHub Action erstellen, die bei jedem Push auf dem `main` Branch ein neues Image in der Registry hochlädt. Dazu erstellen wir eine neue Datei namens `.github/workflows/docker.yml` mit folgendem Inhalt:

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

Die GitHub Action wird bei jedem Push auf dem `main` Branch ausgeführt. Sie baut das Image und lädt es in die Registry hoch.

Weitere Informationen zu GitHub Actions findest du [hier](Material/Task-Task-Bonus/task-bonus.md).

---

## Unterm Strich

In diesem Talk haben wir uns mit den Grundlagen von Docker beschäftigt. Wir haben gesehen, wie Docker verwendet wird, um Container zu erstellen und zu verwalten.

Was kannst Du nach dem Talk tun, damit etwas hängen bleibt?

1. Erstell Dir Docker images für ein aktuelles Projekt
2. Bau Dir eine Docker Compose Datei dazu
3. Für Streber: Schau Dir Docker Swarm und Kubernetes an
