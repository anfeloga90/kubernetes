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
Podemos poner todas estas variables en un configmap o mapas de configuracion

```properties
MYSQL_ROOT_PASSWORD=kubernetes
MYSQL_USER=usudb
MYSQL_PASSWORD=usupass
MYSQL_DATABASE=kubernetes
```

```yaml
MYSQL_ROOT_PASSWORD=kubernetes
MYSQL_USER=usudb
MYSQL_PASSWORD=usupass
MYSQL_DATABASE=kubernetes
```








```sh

```
```yaml

```