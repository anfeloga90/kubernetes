# Service
En pocas palabras, los "servicios" en Kubernetes (K8s) son una abstracción que permite la comunicación entre diferentes conjuntos de pods en un clúster.

Un servicio actúa como un punto de entrada estable para acceder a los pods que forman parte de una aplicación o servicio en Kubernetes. Proporciona una dirección IP y un nombre de DNS para acceder a los pods, independientemente de su ubicación o cambios en su estado.

Cuando creas un servicio, puedes especificar un conjunto de pods a los que deseas acceder mediante etiquetas o selectores. El servicio redirige el tráfico hacia esos pods, ya sea de forma equilibrada (distribuyendo la carga) o seleccionando un pod específico.

Los servicios también ofrecen características de descubrimiento de servicios. Esto significa que, si agregas o eliminas pods, el servicio automáticamente se ajusta para incluir o excluir los pods actualizados, sin que los clientes tengan que preocuparse por realizar cambios en su configuración.

En resumen, los servicios en Kubernetes proporcionan un punto de acceso estable y flexible para acceder a conjuntos de pods. Permiten la comunicación entre diferentes partes de una aplicación o servicio, sin importar dónde se encuentren los pods y sin necesidad de que los clientes conozcan detalles específicos sobre su ubicación.

## tipos de service
ClusterIP: Este es el tipo de servicio predeterminado. Asigna un IP virtual interno (sólo accesible desde dentro del clúster) a un conjunto de pods. Es útil para la comunicación interna entre componentes de la aplicación dentro del clúster.

NodePort: Expone el servicio en un puerto estático en cada nodo del clúster. El tráfico se enruta desde ese puerto hacia el servicio y los pods correspondientes. Puede ser útil cuando necesitas acceder al servicio desde fuera del clúster, pero ten en cuenta que el puerto estándar para este tipo de servicio está en el rango de 30000-32767.

LoadBalancer: Crea un balanceador de carga externo para el servicio. Dependiendo de la plataforma en la que se esté ejecutando Kubernetes, se creará un balanceador de carga en la nube (como un Load Balancer de AWS o un balanceador de carga de tipo servicio en GCP) para distribuir el tráfico entrante a través de los nodos del clúster.

ExternalName: Este tipo de servicio no expone pods, sino que se utiliza para hacer referencia a servicios externos fuera del clúster mediante un nombre de DNS externo. Es útil cuando necesitas acceder a servicios externos sin modificar tu aplicación.

## Comandos

```sh
kubectl create deployment apache1 --image=httpd
kubectl expose deploy apache1 --port=80 --type=NodePort
```

# Crear service
```sh
kubectl create deployment apache1 --image=httpd
deployment.apps/apache1 created

kubectl get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/apache1-6c5f6cf456-pxzpn   1/1     Running   0          43s

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/apache1      NodePort    10.105.136.178   <none>        80:30212/TCP   39s
service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        63d

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/apache1   1/1     1            1           43s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/apache1-6c5f6cf456   1         1         1       43s


kubectl expose deploy apache1 --port=80 --type=NodePort
service/apache1 exposed

kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
apache1      NodePort    10.105.136.178   <none>        80:30212/TCP   65s

curl localhost:30212
<html><body><h1>It works!</h1></body></html>
```









```yaml

```

```sh

```