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









```sh

```