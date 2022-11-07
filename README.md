Español? [Aquí](README.es.md)

# hello-node-in-minikube

Creating a local mini[minikube](https://minikube.sigs.k8s.io/)kube environment and deploying a hello-world nodejs app in it.

## Prerequisites

minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

The installation of minikube is simple. For example on Linux it simply consists of the following 2 commands:

´´´console
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
´´´

Details and other OS setups [here](https://minikube.sigs.k8s.io/docs/start/).

The second requirement is the command line tool for Kubernetes [kubeclt](https://kubernetes.io/docs/reference/kubectl/kubectl/). To install it follow the steps seen [here](https://kubernetes.io/docs/tasks/tools/). It is possible to avoid the installation and delegate the execution of kubectl to minikube. For example in Linux it is possible to do it with an alias:

´´´console
alias kubectl="minikube kubectl --"
´´´
