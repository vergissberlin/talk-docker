# Workshop - Von Docker bis Kubertes

## Kubernetes

### MiniKube vs Docker Desktop vs Rancher Desktop vs Kind

Es gibt verschiedene Möglichkeiten Kubernetes lokal zu installieren. Wir werden uns hier auf die Installation von MiniKube konzentrieren.

Möchtest Du mehr über die jeweiligen Vor- und Nachteile erfahren, kannst Du gerne [hier](https://itnext.io/goodbye-docker-desktop-hello-minikube-3649f2a1c469
) nachlesen.

#### Docker Desktop

* Lokale Installation von Kubernetes
* Lokale Installation von Docker
* Verwendung von Docker Images
* Verwaltung von Kubernetes über Docker Desktop
* TBD

##### Installation von Docker Desktop

1. Installation von [Docker Desktop](https://www.docker.com/products/docker-desktop)

#### Rancher Desktop

* Lokale Installation von Kubernetes
* Lokale Installation von Docker
* TBD

#### MiniKube

* Lokale Installation von Kubernetes
* TBD

##### Installation von MiniKube

1. Installation von [minikube](https://minikube.sigs.k8s.io/docs/start/)
2. Shortcut für minikube in der Shell konfigurieren (z.B. `alias minikube="minikube.exe"`)
3. Installation von [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
4. Shortcut für kubectl in der Shell konfigurieren (z.B. `alias kubectl="kubectl.exe"`)

##### Kubernetes Cluster starten

1. Starten des Kubernetes Clusters mit `minikube start`

##### Kubernetes Dashboard

1. Starten des Kubernetes Dashboard mit `minikube dashboard`

### Starten eines Pods

1. Erstellen eines Pods mit `kubectl run hello-world --image=hello-world`
2. Überprüfen des Pods mit `kubectl get pods`
3. Abstraktion von Pods mit `kubectl get deployments`
4. Anweisungen für Pods mit `kubectl describe pods`
