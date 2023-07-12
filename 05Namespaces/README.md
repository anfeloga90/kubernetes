# Namespaces
En pocas palabras, los "namespaces" en Kubernetes (K8s) son una forma de crear espacios virtuales o particiones lógicas dentro de un clúster de Kubernetes.

Un namespace te permite dividir y aislar lógicamente los recursos de Kubernetes en grupos separados. Puedes pensar en ellos como directorios virtuales que ayudan a organizar y administrar los objetos de Kubernetes, como pods, servicios y volúmenes.

Cada objeto de Kubernetes pertenece a un namespace específico. Esto evita colisiones de nombres y permite tener múltiples instancias de recursos con el mismo nombre en diferentes namespaces.

Los namespaces también proporcionan aislamiento y control de acceso a los recursos. Puedes establecer políticas y permisos de acceso para cada namespace, lo que permite a diferentes equipos o usuarios trabajar de forma independiente en su propio espacio sin interferir con otros.

Además, los namespaces facilitan la gestión y el monitoreo de los recursos en un clúster de Kubernetes, ya que puedes filtrar y visualizar los recursos en función de su namespace.

En resumen, los namespaces en Kubernetes son espacios virtuales que permiten la división lógica y el aislamiento de los recursos en un clúster. Ayudan a organizar, administrar y aislar los objetos de Kubernetes, así como a establecer políticas y permisos específicos para cada grupo de recursos.


## Comandos

```sh
kubectl create namespace n1
namespace/n1 created

kubectl get namespace
NAME              STATUS   AGE
default           Active   64d
kube-node-lease   Active   64d
kube-public       Active   64d
kube-system       Active   64d
n1                Active   11s
```

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev1
  labels:
     tipo: desarrollo

```

## Establecer un namespace por defecto
podemos editamos el fichero .kube o usar el siguiente comando

```sh
kubectl config set-context --current --namespace=n1
Context "docker-desktop" modified.
```

## Poner Cpu y memoria a los namespaces
max y min se usa para el total de componentes del namespace
default y defaultRequest son los recursos por defecto que se asignan a cada pod

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: recursos
spec:
  limits:
  - default:
      memory: 512Mi
      cpu: 1
    defaultRequest:
      memory: 256Mi
      cpu: 0.5
    max:
      memory: 1Gi
      cpu: 4
    min:
      memory: 128Mi
      cpu: 0.5
    type: Container

```

```sh
kubectl apply -f limitex.yaml
limitrange/recursos created

kubectl describe namespace   
Name:         default
Labels:       kubernetes.io/metadata.name=default
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


Name:         kube-node-lease
Labels:       kubernetes.io/metadata.name=kube-node-lease
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


Name:         kube-public
Labels:       kubernetes.io/metadata.name=kube-public
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


Name:         kube-system
Labels:       kubernetes.io/metadata.name=kube-system
Annotations:  <none>
Status:       Active

No resource quota.

No LimitRange resource.


Name:         n1
Labels:       kubernetes.io/metadata.name=n1
Annotations:  <none>
Status:       Active

No resource quota.

Resource Limits
 Type       Resource  Min    Max  Default Request  Default Limit  Max Limit/Request Ratio
 ----       --------  ---    ---  ---------------  -------------  -----------------------
 Container  cpu       500m   4    500m             1              -
 Container  memory    128Mi  1Gi  256Mi            512Mi          -

```

## Controlar eventos dentro de un namespace

```sh
kubectl get events
LAST SEEN   TYPE      REASON              OBJECT                          MESSAGE
13m         Normal    Scheduled           pod/elastic-8448d79947-4ch95    Successfully assigned n1/elastic-8448d79947-4ch95 to docker-desktop
12m         Normal    Pulling             pod/elastic-8448d79947-4ch95    Pulling image "elasticsearch:7.6.0"
12m         Warning   Failed              pod/elastic-8448d79947-4ch95    Failed to pull image "elasticsearch:7.6.0": rpc error: code = Unknown desc = no matching manifest for linux/arm64/v8 in the manifest list entries
12m         Warning   Failed              pod/elastic-8448d79947-4ch95    Error: ErrImagePull
12m         Normal    BackOff             pod/elastic-8448d79947-4ch95    Back-off pulling image "elasticsearch:7.6.0"
12m         Warning   Failed              pod/elastic-8448d79947-4ch95    Error: ImagePullBackOff
13m         Normal    Scheduled           pod/elastic-8448d79947-whhgr    Successfully assigned n1/elastic-8448d79947-whhgr to docker-desktop
12m         Normal    Pulling             pod/elastic-8448d79947-whhgr    Pulling image "elasticsearch:7.6.0"
12m         Warning   Failed              pod/elastic-8448d79947-whhgr    Failed to pull image "elasticsearch:7.6.0": rpc error: code = Unknown desc = no matching manifest for linux/arm64/v8 in the manifest list entries
12m         Warning   Failed              pod/elastic-8448d79947-whhgr    Error: ErrImagePull
12m         Normal    BackOff             pod/elastic-8448d79947-whhgr    Back-off pulling image "elasticsearch:7.6.0"
12m         Warning   Failed              pod/elastic-8448d79947-whhgr    Error: ImagePullBackOff
13m         Normal    SuccessfulCreate    replicaset/elastic-8448d79947   Created pod: elastic-8448d79947-whhgr
13m         Normal    SuccessfulCreate    replicaset/elastic-8448d79947   Created pod: elastic-8448d79947-4ch95
13m         Normal    ScalingReplicaSet   deployment/elastic              Scaled up replica set elastic-8448d79947 to 2
```