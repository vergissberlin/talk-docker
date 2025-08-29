# 💬 Talk: Docker for beginners

```text
Dauer:              1h 30min - 2h
Niveau:             Einsteiger (keine Vorkenntnisse notwendig)
Zielgruppe:         Einsteiger, die Docker kennenlernen wollen
Voraussetzungen:    Laptop mit Internetzugang und Docker installiert
Sprache:            Deutsch
Author:             André Lademann <vergissberlin@gmail.com>
```

## Goal of the talk

At the end of the talc you show how you are fundamentally handled with docker. You can pack applications in Docker images and execute, debug and manage them. If everything goes well, you can at the end you can upload your own docker image to Docker Hub and share it with others.

* * *

## Terms from the docker universe

### Docker

Docker is software that enables applications to be carried out in containers. These containers are lightweight and contain everything you need to run. They can be carried out on any operating system on which docker is installed.

#### Docker Image

A docker image is a template used to create containers. An image contains everything a[Container](#docker-container)Need to run. An image can consist of one or more layers. Each layer contains a number of instructions that are carried out when creating a container. If an image contains several layers, the last layer is used as the base and the previous layers are added as overlay.

#### Docker Container

A container is a executable instance of a[Docker Images](#docker-image). A container is an isolated environment that consists of a series of layers. Each layer contains a number of instructions that are carried out when creating a container. If a container contains several layers, the last layer is used as the base and the previous layers are added as an overlay.

#### Dockerfile

A docker file is a file that contains instructions that Docker uses to a[Docker Image](#docker-image)to create. A docker file contains a number of instructions that are designed when creating an image. If a docker file contains several instructions, the last instruction is used as the basis and the previous instructions are added as overlay.

##### Example docker file

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Docker Compose

Docker compose is a tool with which you can use application environments with several[Docker containers](#docker-container)Based on definitions defined in a Yaml file. He uses service definitions to build up fully customizable environments with several containers that[Networks](#docker-netzwerke)and[Data volumes](#docker-volumes)can share.

#### Example docker compose

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

### Docker networks

Docker networks are a way to connect containers. Docker offers three different types of networks:`bridge`,`host`and`none`.`bridge`is the standard mode in which Docker container connects.`host`Use the host's host network on which the container is executed.`none`Deactivates the network for the container.

#### Example docker compose with network

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

Docker Volumes are a way to share data between containers. Docker offers two different types of volumes:`bind`and`volume`.`bind`binds a directory on the host to a directory in the container.`volume`Creates a volume that is managed by Docker.

#### Example docker compose with volume

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

### Preparation

Check the following things to prepare:

-   [ ] Docker installed and run -up`docker version`
-   [ ] Docker compose installed and run -up`docker compose version`
-   [ ] You are at[Docker Hub](https://hub.docker.com/)registered`docker login`
-   [ ] **Optional:**Press for this repo to the star ⭐ in the upper right corner: D
-   [ ] Fork (e) this repositories on github and clone (e) it on your computer

### Tasks

**The goal of the tasks**Is it one[Docker Image](#docker-image)To create that carries out a simple web application. The web application should output a simple HTML page. Then we will start the Docker Image locally and see how we can call up the application via the browser.

1.  Creating a docker file
2.  Start of a container
3.  Logging des Containers mit`docker logs`
4.  Call up a website in the browser
5.  Debugging des Containers mit`docker exec`
6.  Restart of the container
7.  Stop the containers
8.  Use volume mounts
9.  Publish your own image in the registry
10. Creating a Docker Compose File
11. Starting, stopping and debugging an application with Docker Compose
12. **Bonus:**Establishment of a Github Action that automatically builds the image and in the registry Push

### Task 1 - Creating a docker file

First of all, we want to look at a simple Docker image. To do this, we create a directory and create a file called`Dockerfile`With the following content:

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y cowsay
CMD ["cowsay", "Hello World"]
```

### Task 2 - Start of a container

Now we can start building the image. To do this, we switch to the directory and carry out the following command:

```bash
docker build -t workshop .
```

The`-t`Flag gives the image a name. The point at the end of the command indicates that the docker file lies in the current directory.

Now we can start the container:

```bash
docker run workshop
```

### Task 3 - Logging of the container with`docker logs`

We can use the log of the container with the command`docker logs`display. To do this, we need the container ID, which we with the command`docker ps`can display.

```bash
docker logs <container-id>
```

### Task 4 - Calling a website in the browser

We need a web server for a website. To do this, we create a new file called`Dockerfile`With the following content:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

So that we can access the website in the browser, we create a new file called`index.html`With the following content:

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

The`-p`Flag binds port 80 of the container to the port 8000 of the host. The`-d`Flag starts the container in the background.

### Exercise 5 - Debugging of the container with`docker exec`

We can use the content of the container with the command`docker exec`display. To do this, we need the container ID, which we with the command`docker ps`can display.

```bash
docker exec -it <container-id> bash
```

### Task 6 - restart of the container

We can use the container with the command`docker restart`start anew. To do this, we need the container ID, which we with the command`docker ps`can display.

```bash
docker restart <container-id>
```

### Task 7 - Stop the container

We can use the container with the command`docker stop`stop. To do this, we need the container ID, which we with the command`docker ps`can display.

```bash
docker stop <container-id>
```

### Task 8 - Use of Volume Mounts

We can use the container with the command`docker run`with the`-v`Start flag. To do this, we need the path to the directory on the host and the path in the container.

```bash
docker run -v $PWD:/var/www/html workshop
```

Die Variable`$PWD`Specifies the path to the current directory.
According to the colon, the path is in the container.

### Task 9 - Publish your own image in the registry

We can use our own image with the command`docker push`Upload to the registry. To do this, we need the name of the image and the day.

```bash
docker push <image-name>:<tag>
```

The day is optional. If it is not specified,`latest`used.

### Task 10 - Creating a Docker Compose File

We can have several containers with the command`docker run`start. To do this, we need the names of the images and the ports.

```bash
docker run -p 8000:80 workshop
docker run -p 8001:80 workshop
```

We can also create a Docker compose file. To do this, we create a new file called`docker-compose.yml`With the following content:

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

You can find wide material on the subject[here](Material/Task-Task-10/task-10.md).

### Task 11 - Start an application with a docker compose

We can use the command`docker-compose up`The start.

```bash
docker-compose up -d
```

The`-d`Flag starts the application like Docker in the background.

**Here is a list of the most important commands:**

```bash
docker-compose up -d # Startet die Anwendung im Hintergrund
docker-compose down # Stoppt die Anwendung
docker-compose ps # Zeigt die laufenden Container an
docker-compose logs # Zeigt die Logs der Container an
docker-compose exec <service> bash # Startet eine Shell im Container
```

### Task 12 - Setup of a Github Action

We can create a github action that is on the`main`Branch a new image in the registry Hochlädt. To do this, we create a new file called`.github/workflows/docker.yml`With the following content:

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

The Github Action is on the`main`Branch executed. She builds the image and invites it to the registry.

You can find more information about Github Actions[here](Material/Task-Task-Bonus/task-bonus.md).

* * *

## Beside

In this talk we dealt with the basics of Docker. We saw how Docker is used to create and manage containers.

What can you do for the talk so that something gets stuck?

1.  Create Docker Images for a current project
2.  Build a docker compose file
3.  For Streber: Take a look at Docker Swarm and Kubernetes

## Contribute

Do you have suggestions for improvement? Then like to create a pull request or write a few lines in[Forum for discussion](https://github.com/vergissberlin/talk-docker/discussions)or[Twitter](https://twitter.com/vergissberlin).
