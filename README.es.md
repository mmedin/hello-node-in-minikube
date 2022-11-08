English? [Here](README.md)

# hello-node-in-minikube

Creando un entorno [minikube](https://minikube.sigs.k8s.io/) local y desplegando una aplicación nodejs "hola-mundo".

(basado en [este tutorial](https://kubernetes.io/es/docs/tutorials/hello-minikube/) oficial)

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

## Empaquetar la app en un container

Dentro de este repo, en la carpeta [hello-node](hello-node/) vas a encontrar 2 archivos con el código suficiente para construir una app nodejs básica (package.json y server.js).

Antes de desplegar esta aplicación en nuestro "cluster" minikube debemos realizar un build para obtener un container que tenga la app y sus dependencias.

```bash
cd hello-node
docker build -t hello-node:1.0.0 .
cd ..
```

Esto generará un container local que podemos ver con `docker images`:

```console
# docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
hello-node   1.0.0     f811a976efe9   10 minutes ago   122MB
```

## Levantar un clúster minikube

Sencillamente ejecutamos:

```console
# minikube start
...
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Minikube puede utilizar distintos drivers para resolver la virtualización necesaria para levantar el clúster. Por defecto, si Docker está instalado en el sistema, ulitiza ese driver. Si no funciona bien se pueden probar otras opciones, como por ejemplo VirtualBox (que debe estar instalado, por supuesto). En tal caso, el comando sería el siguiente:

```bash
minikube start --driver=virtualbox
```

Para ver que todo quedó funcionando bien:

```console
# minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

También para verificar la instalación y contar con una UI web donde ver los recursos del clúster, podemos ejecutar lo siguiente:

```bash
minikube dashboard
```

(ctrl-c para volver al prompt)

## Crear un Deployment

Un [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) en Kubernetes es un grupo de uno o más contenedores, asociados por propósito y que comparten almacenamiento y red. El Pod en este tutorial tiene solo un contenedor.

Antes de crear el deployment debemos "darle" a minikube el container que creamos localmente con Docker. En un contexto real, debiéramos haber subido el container a algún servicio de registry, pero en esta práctica simplificada sencillamente lo trabajamos de manera local:

```bash
minikube image load hello-node:1.0.0
```

Verificamos que la imagen está disponible:

```console
# minikube image ls
...
docker.io/library/hello-node:1.0.0
...
```

Ahora sí, vamos al deployment. Normalmente ejecutaríamos lo siguiente:

```bash
kubectl create deployment hello-node --image=hello-node:1.0.0
```

pero en nuestro caso necesitamos un paso adicional para evitar que minikube busque la imagen en la registry en lugar de hacerlo localmente. O sea que ejecutamos:

```bash
kubectl create deployment hello-node --image=hello-node:1.0.0 -o yaml --dry-run > hello-node-deployment.yaml
```

para bajar a un archivo el YAML con la definición, y lo editamos para agrelarle el parámetro `imagePullPolicy: Never`:

```console
# vim hello-node-deployment.yaml
...
    spec:
      containers:
      - image: hello-node:1.0.0
        name: hello-node
        resources: {}
        imagePullPolicy: Never
...
```

Y finalmente aplicamos el YAML para crear el deployment:

```bash
kubectl apply -f hello-node-deployment.yaml
```

Podemos verificar que está arriba nuestro deployment y el pod asociado:

```console
# kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   1/1     1            1           8s
# kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-6c8fb57d94-8xr4s   1/1     Running   0          16s
```

## Crear un Service

Para lograr que el contenedor `hello-node` sea accesible desde afuera de la red virtual Kubernetes, se debe exponer el Pod como un [Service](https://kubernetes.io/docs/concepts/services-networking/service/) de Kubernetes:

```bash
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

Verificando:

```console
# kubectl get services
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   10.106.92.22   <pending>     8080:32004/TCP   40s
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP          4h52m
```

Hemos llegado al final, y como resultado tenemos que poder ver nuestra app funcionando en el browser:

```bash
minikube service hello-node
```
