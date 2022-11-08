Español? [Aquí](README.es.md)

# hello-node-in-minikube

Creating a local [minikube](https://minikube.sigs.k8s.io/) environment and deploying a hello-world nodejs app in it.

## Prerequisites

minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

The installation of minikube is simple. For example on Linux it simply consists of the following 2 commands:

```console
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Details and other OS setups [here](https://minikube.sigs.k8s.io/docs/start/).

The second requirement is the command line tool for Kubernetes [kubeclt](https://kubernetes.io/docs/reference/kubectl/kubectl/). To install it follow the steps seen [here](https://kubernetes.io/docs/tasks/tools/). It is possible to avoid the installation and delegate the execution of kubectl to minikube. For example in Linux it is possible to do it with an alias:

```console
alias kubectl="minikube kubectl --"
```

Finally, we need [Docker](https://www.docker.com/) to perform the build and get an image containing our sample application. Installation details [here](https://www.docker.com/get-started/)

## We pack the app in a container

Inside this repo, in the [hello-node](hello-node/) folder you will find 2 files with enough code to build a basic nodejs app (package.json and server.js).

Before deploying this application in our minikube "cluster", we must perform a build to obtain a container with the app and its dependencies.

```bash
cd hello-node
docker build -t hello-node:1.0.0 .
```

This will generate a local container. We can see it with `docker images`:

```console
# docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
hello-node   1.0.0     f811a976efe9   10 minutes ago   122MB
```
