English? [Here](README.md)

# hello-node-in-minikube

Creando un entorno [minikube](https://minikube.sigs.k8s.io/) local y desplegando una aplicación nodejs "hola-mundo".

## Pre-requisitos

minikube es Kubernetes local, con el objetivo de facilitar el aprendizaje y el desarrollo en Kubernetes.

La instalación de minikube es sencilla. Por ejemplo en Linux consisten simplemente en los siguientes 2 comandos:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Detalles y configuración para otros SO [aquí](https://minikube.sigs.k8s.io/docs/start/).

El segundo requerimiento es la herramienta de línea de comandos para Kubernetes [kubeclt](https://kubernetes.io/docs/reference/kubectl/kubectl/). Para instalarla seguir los pasos que se ven [aquí](https://kubernetes.io/docs/tasks/tools/). Se puede evitar la instalación y delegarle a minikube la ejecución de kubectl. Por ejemplo en Linux se logra con un alias:

```bash
alias kubectl="minikube kubectl --"
```

Finalmente, necesitamos [Docker](https://www.docker.com/) para realizar el build y obtener una imagen que contenga nuestra aplicación de ejemplo. Detalles sobre la instalación [aquí](https://www.docker.com/get-started/)

## Empaquetamos la app en un container

Dentro de este repo, en la carpeta [hello-node](hello-node/) vas a encontrar 2 archivos con el código suficiente para construir una app nodejs básica (package.json y server.js).

Antes de desplegar esta aplicación en nuestro "cluster" minikube debemos realizar un build para obtener un container que tenga la app y sus dependencias.

```bash
cd hello-node
docker build -t hello-node:1.0.0 .
```

Esto generará un container local que podemos ver con `docker images`:

```console
# docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
hello-node   1.0.0     f811a976efe9   10 minutes ago   122MB
```
