# Variables - ConfigMaps - Secrets

## variables
las variables quedab definidas como variables de entorno dentro del contenedor

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deploy
  labels:
    app: mysql
    type: db
spec:
  replicas: 1
  selector: 
    matchLabels:
      app: mysql
      type: db
  template:
    metadata:
      labels:
        app: mysql
        type: db
    spec:
      containers:
        - name: mysql57
          #image: mysql:5.7
          image: mariadb:10.5.8
          ports:
            - containerPort: 3306
              name: db-port
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: kubernetes
            - name: MYSQL_USER
              value: usudb
            - name: MYSQL_PASSWORD
              value: usupass
            - name: MYSQL_DATABASE
              value: kubernetes
```

```sh
kubectl apply -f mysql.yaml 
deployment.apps/mysql-deploy created

k get pods
NAME                            READY   STATUS    RESTARTS   AGE
mysql-deploy-69d6d5ff4c-cqq8q   1/1     Running   0          48s

k exec -ti mysql-deploy-69d6d5ff4c-cqq8q -- env     
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=mysql-deploy-69d6d5ff4c-cqq8q
TERM=xterm
MYSQL_ROOT_PASSWORD=kubernetes
MYSQL_USER=usudb
MYSQL_PASSWORD=usupass
MYSQL_DATABASE=kubernetes
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
GOSU_VERSION=1.12
GPG_KEYS=177F4010FE56CA3336300305F1656F24C74CD1D8
MARIADB_MAJOR=10.5
MARIADB_VERSION=1:10.5.8+maria~focal
HOME=/root


k exec -ti mysql-deploy-69d6d5ff4c-cqq8q -- mysql -u root -pkubernetes 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| kubernetes         |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

MariaDB [(none)]> 
```

## ConfigMaps

En Kubernetes, un "ConfigMap" es un objeto que se utiliza para almacenar datos de configuración en forma de pares clave-valor. Un ConfigMap proporciona una forma de separar la configuración de una aplicación de su código fuente y permite una mayor flexibilidad y gestión de la configuración.

Los ConfigMaps son útiles cuando necesitas proporcionar parámetros de configuración a tus aplicaciones sin tener que modificar o reconstruir los contenedores. Algunos ejemplos de datos de configuración comunes incluyen variables de entorno, archivos de configuración o cualquier otro valor que tu aplicación pueda necesitar.

Un ConfigMap puede crearse a partir de archivos o a través de comandos en línea. Una vez creado, puedes montar el ConfigMap como un volumen en los pods de tu aplicación o inyectar sus valores como variables de entorno en los contenedores.

La ventaja de utilizar ConfigMaps es que te permite mantener la configuración de tu aplicación de forma más modular y portable. Puedes actualizar o modificar los valores del ConfigMap sin tener que modificar directamente los archivos o las definiciones de los pods, lo que facilita la gestión y la flexibilidad de la configuración en Kubernetes.

En resumen, un ConfigMap en Kubernetes es un objeto que almacena datos de configuración como pares clave-valor. Permite separar la configuración de la aplicación del código fuente y proporciona una forma flexible de proporcionar parámetros de configuración a través de variables de entorno o volúmenes en los pods de tu aplicación.

Podemos poner todas estas variables en un configmap o mapas de configuracion
game.properties
```properties
MYSQL_ROOT_PASSWORD=kubernetes
MYSQL_USER=usudb
MYSQL_PASSWORD=usupass
MYSQL_DATABASE=kubernetes
```

pod1.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  containers:
    - name: test-container
      image: gcr.io/google-samples/node-hello:1.0
      env:
        # Define the environment variable
        - name: DATOS_MYSQL
          valueFrom:
            configMapKeyRef:
              name: datos-mysql-env
              key: datos_mysql.properties
  restartPolicy: Never
``` 

```sh
k create configmap datos-mysql-env --from-env-file datos_mysql.properties 
configmap/datos-mysql-env created

k get cm 
NAME               DATA   AGE
datos-mysql-env    4      115s
kube-root-ca.crt   1      64d

k describe cm datos-mysql-env
Name:         datos-mysql-env
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
MYSQL_DATABASE:
----
kubernetes
MYSQL_PASSWORD:
----
usupass
MYSQL_ROOT_PASSWORD:
----
kubernetes
MYSQL_USER:
----
usudb

BinaryData
====

Events:  <none>

k apply -f pod1.yaml 
pod/pod1 created
```

datos-mysql-env.yaml
```yaml
apiVersion: v1
data:
  MYSQL_DATABASE: kubernetes
  MYSQL_PASSWORD: usupass
  MYSQL_ROOT_PASSWORD: kubernetes
  MYSQL_USER: usudb
kind: ConfigMap
metadata:
  name: datos-mysql-env
  namespace: default
```


```sh
k apply -f datos-mysql-env.yaml
configmap/datos-mysql-env created

k  get cm
NAME               DATA   AGE
datos-mysql-env    4      15s
```

mysql-env.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deploy
  labels:
    app: mysql
    type: db
spec:
  replicas: 1
  selector: 
    matchLabels:
      app: mysql
      type: db
  template:
    metadata:
      labels:
        app: mysql
        type: db
    spec:
      containers:
        - name: mysql57
          image: mysql:5.7
          ports:
            - containerPort: 3306
              name: db-port
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: datos-mysql-env
                  key: MYSQL_ROOT_PASSWORD

            - name: MYSQL_USER
              valueFrom:
                configMapKeyRef:
                  name: datos-mysql-env
                  key: MYSQL_USER
            
            - name: MYSQL_DATABASE
              valueFrom:
                configMapKeyRef:
                  name: datos-mysql-env
                  key: MYSQL_DATABASE

            - name: MYSQL_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: datos-mysql-env
                  key: MYSQL_PASSWORD
```

## configmaps y volumenes

Crear las variables en un volumen

configmap.yaml
pod1 (2).yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-volumen
  namespace: default
data:
  ENTORNO: "desarrollo"
  VERSION: "1.0"
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  containers:
    - name: contenedor1
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "sleep 1000000" ]
      volumeMounts:
      - name: volumen-config-map
        mountPath: /etc/config-map
  volumes:
    - name: volumen-config-map
      configMap:
        name: config-volumen
  restartPolicy: Never
```

```sh
kubectl apply -f configmap.yaml 
configmap/config-volumen created

kubectl apply -f pod1\ \(2\).yaml 
pod/pod1 created

k exec -ti pod1 -- ls /etc/config-map
ENTORNO  VERSION

k exec -ti pod1 -- cat /etc/config-map/ENTORNO
desarrollo                                            

k exec -ti pod1 -- cat /etc/config-map/VERSION
1.0
```

## Secrets

En Kubernetes, un "secreto" es un objeto que se utiliza para almacenar y gestionar información sensible, como contraseñas, claves de API, tokens de acceso u otros datos confidenciales que las aplicaciones necesitan para funcionar.

Los secretos en Kubernetes están diseñados para mantener la información sensible separada de las definiciones de las aplicaciones y los contenedores. Esto ayuda a garantizar la seguridad y facilita la gestión de secretos en un entorno de clúster.

Los secretos en Kubernetes se almacenan en el clúster de forma encriptada y se pueden montar en los contenedores de los pods como volúmenes o variables de entorno. De esta manera, los contenedores pueden acceder a los secretos y utilizar la información sensible sin necesidad de tenerla explícitamente en su configuración.

El uso de secretos permite que los datos confidenciales se mantengan protegidos y no se expongan en archivos de configuración o en código fuente. Además, Kubernetes proporciona mecanismos para administrar y rotar los secretos de forma segura, lo que es importante para garantizar la seguridad a largo plazo de las aplicaciones.

En resumen, un secreto en Kubernetes es un objeto utilizado para almacenar y gestionar información sensible de manera segura, como contraseñas o claves de API. Permite separar y proteger la información confidencial de las definiciones de las aplicaciones y facilita su acceso desde los contenedores de forma segura.




```txt
Esto es un ejemplo de secrets
incorporaos desde fichero
dentro de un contenedor
```

```sh
kubectl create secret generic datos --from-file=datos.txt
secret/datos created

kubectl get secrets
NAME    TYPE     DATA   AGE
datos   Opaque   1      21s

kubectl describe secrets datos
Name:         datos
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
datos.txt:  83 bytes


kubectl get secret datos -o yaml
apiVersion: v1
data:
  datos.txt: RXN0byBlcyB1biBlamVtcGxvIGRlIHNlY3JldHMNCmluY29ycG9yYW9zIGRlc2RlIGZpY2hlcm8NCmRlbnRybyBkZSB1biBjb250ZW5lZG9yDQo=
kind: Secret
metadata:
  creationTimestamp: "2023-07-13T03:51:10Z"
  name: datos
  namespace: default
  resourceVersion: "576957"
  uid: 1f3db9af-f0ac-4cae-b494-59e0818a68a0
type: Opaque
```


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  containers:
    - name: test-container
      image: ubuntu
      command: [ "/bin/sh", "-c", "sleep 1000000" ]
      env:
        - name: DATOS
          valueFrom:
            secretKeyRef:
              name: datos
              key: datos.txt
  restartPolicy: Never
```


```sh
k apply -f pod2.yaml 
pod/pod1 created

k exec -ti pod1 -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=pod1
TERM=xterm
DATOS=Esto es un ejemplo de secrets
incorporaos desde fichero
dentro de un contenedor

KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
HOME=/root
```

