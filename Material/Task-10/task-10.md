
# Docker Compose

Was ist *Docker Compose*? Siehe in der [README](../README.md#docker-compose).

## docker-compose.yml erstellen

Die *docker-compose.yml* ist eine Datei (die hat imer Recht), welche die Anweisungen für das Starten von mehreren Docker Images enthält. Die Anweisungen werden von oben nach unten ausgeführt. Die Anweisungen werden in der Regel in Kleinbuchstaben geschrieben.

## Beispiel für eine docker-compose.yml

```yaml
version: "3.9"

services:
  web:
    image: vergissberlin/talk-docker:latest
    build: .
    ports:
      - "8080:80"
    depends_on:
      - db
      - redis
    networks:
      - talk-docker
  db:
    image: postgres:latest
    environment:
      POSTGRES_PASSWORD: password
    networks:
        - talk-docker
  redis:
    image: redis:latest
    networks:
        - talk-docker

networks:
  default:
    external:
      name: talk-docker

```
