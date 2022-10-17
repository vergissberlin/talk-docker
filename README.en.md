# 💬 Talk: Docker for beginners

```text
Dauer:              1h 30min - 2h
Niveau:             Einsteiger (keine Vorkenntnisse notwendig)
Zielgruppe:         Einsteiger, die Docker kennenlernen wollen
Voraussetzungen:    Laptop mit Internetzugang und Docker installiert
Sprache:            Deutsch
Author:             André Lademann <vergissberlin@gmail.com>
```

## goal of the talk

At the end of the talk, you will show how to deal with Docker in a fundamental way. You can package applications into Docker images and run, debug and manage them. If everything goes well, you can end up uploading your own Docker image to Docker Hub and sharing it with others.

* * *

## Terms from the Docker universe

### Docker

Docker is software that allows applications to run in containers. These containers are lightweight and contain everything they need to run. They can be run on any operating system that has Docker installed.

#### Docker Image

A Docker Image is a template that Docker uses to create containers. An image contains everything a[Container](#docker-container)needed to run. An image can consist of one or more layers. Each layer contains a set of instructions that are executed when a container is created. If an image contains multiple layers, the last layer is used as the base and the previous layers are added as an overlay.

#### Docker Container

A container is an executable instance of a[Docker Images](#docker-image). A container is an isolated environment made up of a number of layers. Each layer contains a set of instructions that are executed when a container is created. If a container contains multiple layers, the last layer is used as the base and the previous layers are added as an overlay.

#### Dockerfile

A Dockerfile is a file that contains instructions that Docker uses to build a[Docker Image](#docker-image)to create. A Dockerfile contains a set of instructions that are executed when an image is built. If a Dockerfile contains multiple directives, the last directive is used as the base and the previous directives are added as an overlay.

##### Example Dockerfile

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Docker Compose

Docker Compose is a tool that allows you to create application environments with multiple[Docker containers](#docker-container)based on definitions specified in a YAML file. It uses service definitions to build fully customizable multi-container environments that[networks](#docker-netzwerke)and[data volumes](#docker-volumes)can share.

#### Example Docker Compose

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

### Docker Networks

Docker networks are a way to connect containers together. Docker offers three different types of networks:`bridge`,`host`and`none`.`bridge`is the default mode in which Docker connects containers.`host`uses the host network of the host running the container.`none`disables networking for the container.

#### Example Docker Compose with network

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

Docker Volumes are a way to share data between containers. Docker offers two different types of volumes:`bind`and`volume`.`bind`binds a directory on the host to a directory in the container.`volume`creates a volume managed by Docker.

#### Example Docker Compose with Volume

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

## Workshop

### preparation

Check the following things to prepare:

-   [ ] Docker installed and running`docker version`
-   [ ] Docker Compose installed and running`docker compose version`
-   [ ] you are at[Docker Hub](https://hub.docker.com/)Registered`docker login`
-   [ ] **Optional:**For this repo, press the star ⭐ in the upper right corner :D
-   [ ] Fork(e) this repository on GitHub and clone(e) it to your machine

### tasks

**The goal of the tasks**is it a[Docker Image](#docker-image)to create one that runs a simple web application. The web application should output a simple HTML page. We will then start the docker image locally and see how we can call up the application via the browser.

1.  Creating a Dockerfile
2.  Starting a container
3.  Logging des Containers mit`docker logs`
4.  Calling up a website in the browser
5.  Debugging des Containers mit`docker exec`
6.  Restart the container
7.  Stop the Containers
8.  Using volume mounts
9.  Publish your own image in the registry
10. Creating a Docker Compose File
11. Start, stop and debug an application with Docker Compose
12. **Bonus:**Setting up a GitHub action that automatically builds the image and pushes it to the registry

### Task 1 - Creating a Dockerfile

First, let's look at a simple Docker image. To do this, we create a directory and create a file called`Dockerfile`with the following content:

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Task 2 - Starting a container

Now we can start building the image. To do this, we change to the directory and execute the following command:

```bash
docker build -t workshop .
```

That`-t`Flag gives the image a name. The dot at the end of the command indicates that the Dockerfile is in the current directory.

Now we can start the container:

```bash
docker run workshop
```

### Task 3 - logging of the container with`docker logs`

We can get the log of the container with the command`docker logs`display. To do this, we need the container ID that we use with the command`docker ps`can display.

```bash
docker logs <container-id>
```

### Task 4 - Calling up a web page in the browser

For a website we need a web server. To do this, we create a new file called`Dockerfile`with the following content:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

So that we can call up the website in the browser, we create a new file called`index.html`with the following content:

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

Now we can build the image and start the container:

```bash
docker build -t workshop .
docker run -p -d 8000:80 workshop
```

That`-p`Flag binds the container's port 80 to the host's port 8000. That`-d`Flag starts the container in the background.

### Task 5 - Debugging the container with`docker exec`

We can view the contents of the container using the command`docker exec`display. To do this, we need the container ID that we use with the command`docker ps`can display.

```bash
docker exec -it <container-id> bash
```

### Task 6 - restart the container

We can create the container with the command`docker restart`start anew. To do this, we need the container ID that we use with the command`docker ps`can display.

```bash
docker restart <container-id>
```

### Task 7 - stopping the container

We can create the container with the command`docker stop`to stop. To do this, we need the container ID that we use with the command`docker ps`can display.

```bash
docker stop <container-id>
```

### Task 8 - Using Volume Mounts

We can create the container with the command`docker run`with the`-v`start flag. For this we need the path to the directory on the host and the path in the container.

```bash
docker run -v $PWD:/var/www/html workshop
```

Die Variable`$PWD`Specifies the path to the current directory.
After the colon is the path in the container.

### Task 9 - Publish your own image in the registry

We can create the custom image with the command`docker push`upload to the registry. For this we need the name of the image and the tag.

```bash
docker push <image-name>:<tag>
```

The tag is optional. If not specified, will`latest`used.

### Task 10 - Creating a Docker Compose File

We can add multiple containers with the command`docker run`start. For this we need the names of the images and the ports.

```bash
docker run -p 8000:80 workshop
docker run -p 8001:80 workshop
```

We can also create a Docker Compose File. To do this, we create a new file called`docker-compose.yml`with the following content:

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

You can find more material on the subject[here](Material/Task-Task-10/task-10.md).

### Task 11 - Launching an application using Docker Compose

We can use the application with the command`docker-compose up`beginning.

```bash
docker-compose up -d
```

That`-d`Flag starts the application in the background like Docker.

**Here is a list of the most important commands:**

```bash
docker-compose up -d # Startet die Anwendung im Hintergrund
docker-compose down # Stoppt die Anwendung
docker-compose ps # Zeigt die laufenden Container an
docker-compose logs # Zeigt die Logs der Container an
docker-compose exec <service> bash # Startet eine Shell im Container
```

### Aufgabe 12 - Einrichtung einer GitHub Action

We can create a GitHub Action that will be triggered on every push on the`main`Branch uploads a new image to the registry. To do this, we create a new file called`.github/workflows/docker.yml`with the following content:

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

The GitHub Action is written to the`main` Branch ausgeführt. Sie baut das Image und lädt es in die Registry hoch.

Learn more about GitHub Actions[here](Material/Task-Task-Bonus/task-bonus.md).

* * *

## bottom line

In this talk we covered the basics of Docker. We've seen how Docker is used to create and manage containers.

What can you do after the talk so that something sticks?

1.  Create Docker images for a current project
2.  Build a Docker Compose file
3.  For geeks: Check out Docker Swarm and Kubernetes
