# Deployments
En pocas palabras, el "deployment" en Kubernetes (también conocido como "despliegue") es un objeto que te permite administrar y controlar la ejecución de tus aplicaciones en un clúster de Kubernetes.

Un deployment en Kubernetes define cómo se deben crear y escalar tus aplicaciones en contenedores dentro del clúster. Proporciona una descripción declarativa de los pods (instancias de contenedores) que deseas ejecutar, así como la forma en que deben actualizarse y gestionarse.

Al utilizar un deployment, puedes especificar el número deseado de réplicas de tus pods, definir las características de escalado automático, como el crecimiento o disminución de réplicas según la carga, y realizar actualizaciones y despliegues de manera controlada y sin tiempo de inactividad.

En resumen, un deployment en Kubernetes es una forma de gestionar y controlar la ejecución de tus aplicaciones en contenedores, permitiéndote mantener tus aplicaciones en funcionamiento de manera confiable y escalable en un entorno de clúster.
## Comandos

```sh
kubectl create deployment apache --image=httpd
```

## Crear deployment
```sh
kubectl create deployment apache --image=httpd

kubectl get deployment
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
apache   1/1     1            1           35s

kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
apache-855464645   1         1         1       92s

kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
apache-855464645-6ff5g   1/1     Running   0          2m

```

## ver deployment
```sh
kubectl describe deploy apache
Name:                   apache
Namespace:              default
CreationTimestamp:      Mon, 10 Jul 2023 17:19:09 -0400
Labels:                 app=apache
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=apache
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=apache
  Containers:
   httpd:
    Image:        httpd
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   apache-855464645 (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  6m36s  deployment-controller  Scaled up replica set apache-855464645 to 1

```

deploy_nginx.yaml
```yaml
apiVersion: apps/v1 # i se Usa apps/v1beta2 para versiones anteriores a 1.9.0
kind: Deployment
metadata:
  name: nginx-d
spec:
  selector:   #permite seleccionar un conjunto de objetos que cumplan las condicione
    matchLabels:
      app: nginx
  replicas: 2 # indica al controlador que ejecute 2 pods
  template:   # Plantilla que define los containers
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80

```

```sh
kubectl create -f deploy_nginx.yaml
deployment.apps/nginx-d created

kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
apache-855464645-6ff5g     1/1     Running   0          18m
nginx-d-7759cfdc55-6lqjh   1/1     Running   0          13s
nginx-d-7759cfdc55-hbvx7   1/1     Running   0          13s

kubectl get rs  
NAME                 DESIRED   CURRENT   READY   AGE
apache-855464645     1         1         1       18m
nginx-d-7759cfdc55   2         2         2       19s

kubectl get deploy
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
apache    1/1     1            1           18m
nginx-d   2/2     2            2           22s
```

### Escalar 
```sh
kubectl scale deploy nginx-d --replicas=5
deployment.apps/nginx-d scaled

kubectl get deploy,rs
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/apache    1/1     1            1           29m
deployment.apps/nginx-d   5/5     5            5           11m

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/apache-855464645     1         1         1       29m
replicaset.apps/nginx-d-7759cfdc55   5         5         5       11m
```