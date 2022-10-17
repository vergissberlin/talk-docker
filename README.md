# 💬 Talk: Von Docker bis Kubernetes

> Ein Einsteiger-Workshop für Docker-Neulinge

Dauer:  1h 30min

## Ziel des Talks

Am Ende des Talks weist Du, wie du wie Du grundlegend mit Docker umgehst. Du kannst Anwendungen in Docker-Images packen und diese in einer Kubernetes-Cluster ausführen.

## Begriffe

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

## Kubernetes

Kubernetes ist eine Open-Source-Plattform für die Automatisierung der Bereitstellung, Skalierung und Verwaltung von Containeranwendungen.

### Kubernetes Container

Ein Kubernetes Container ist eine ausführbare Instanz eines Docker Images. Ein Container ist eine isolierte Umgebung, die aus einer Reihe von Schichten besteht. Jede Schicht enthält eine Reihe von Anweisungen, die beim Erstellen eines Containers ausgeführt werden. Wenn ein Container mehrere Schichten enthält, wird die letzte Schicht als Basis verwendet und die vorherigen Schichten als Overlay hinzugefügt.

### Kubernetes Pods

Ein Pod ist eine Gruppe von einem oder mehreren Containern, die gemeinsam ausgeführt werden und die gleiche Speicher- und Netzwerkressource teilen. Ein Pod ist die kleinste Einheit, die in Kubernetes erstellt und verwaltet werden kann.

### Kubernetes Services

Ein Service ist eine Abstraktion, die eine Gruppe von Pods als ein logisches Set darstellt. Services ermöglichen es, Pods zu finden und miteinander zu kommunizieren, ohne dass die Pods sich selbst kennen. Services können auch verwendet werden, um Pods über einen Cluster hinweg zu verteilen.

### Kubernetes Nodes

Ein Node ist eine virtuelle oder physische Maschine, auf der Kubernetes Pods ausgeführt werden. Ein Node kann einen oder mehrere Pods ausführen.

### Kubernetes Clusters

Ein Cluster ist eine Gruppe von Nodes, die zusammen arbeiten, um Containeranwendungen auszuführen. Ein Cluster besteht aus mindestens einem Master-Node und mehreren Worker-Node.

---

## Unterm Strich

In diesem Artikel haben wir uns mit den Grundlagen von Docker und Kubernetes beschäftigt. Wir haben gesehen, wie Docker verwendet wird, um Container zu erstellen und zu verwalten. Wir haben auch gesehen, wie Kubernetes verwendet wird, um Containeranwendungen zu verwalten. Wir haben auch gesehen, wie Docker und Kubernetes zusammenarbeiten.
