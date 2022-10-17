# Bonus: GitHub Actions

Mit Github Actions ist es möglich, automatisierte Workflows zu erstellen. Diese Workflows können auf verschiedene Events reagieren, wie z.B. einem Push auf einen Branch oder einem Pull Request. In diesem Bonus wollen wir einen Workflow erstellen, der bei jedem Push auf den `main` Branch ein neues Docker Image erstellt und auf Docker Hub hochlädt.

## Aufgaben

1. Erstellen eines GitHub Accouts
2. Push des Repositories auf GitHub in den `main` Branch
3. Erstellen eines GitHub Actions Workflows. Siehe [Docker Workflow](https://github.com/marketplace/actions/build-and-push-docker-images)

### GitHub Actions Workflow Beispiel

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
            tags: vergissberlin/talk-docker:latest
            cache-from: type=registry,ref=vergissberlin/talk-docker:latest
            cache-to: type=inline
```
