English? [Here](README.md)

# hello-node-in-minikube

Creando un entorno [minikube](https://minikube.sigs.k8s.io/) local y desplegando una aplicación nodejs "hola-mundo".

## Pre-requisitos

minikube es Kubernetes local, con el objetivo de facilitar el aprendizaje y el desarrollo en Kubernetes.

La instalación de minikube es sencilla. Por ejemplo en Linux consisten simplemente en los siguientes 2 comandos:

´´´console
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
´´´

Detalles y configuración para otros SO [aquí](https://minikube.sigs.k8s.io/docs/start/).

El segundo requerimiento es la herramienta de línea de comandos para Kubernetes [kubeclt](https://kubernetes.io/docs/reference/kubectl/kubectl/). Para instalarla seguir los pasos que se ven [aquí](https://kubernetes.io/docs/tasks/tools/). Se puede evitar la instalación y delegarle a minikube la ejecución de kubectl. Por ejemplo en Linux se logra con un alias:

´´´console
alias kubectl="minikube kubectl --"
´´´
