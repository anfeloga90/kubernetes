# Ejemplo php redis k8s

## comandos

```sh
kubectl apply -f redis-master.yaml 
deployment.apps/redis-master created

kubectl get all
NAME                                READY   STATUS    RESTARTS   AGE
pod/redis-master-567f9dcd4d-zptsz   1/1     Running   0          40s

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis-master   1/1     1            1           40s

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/redis-master-567f9dcd4d   1         1         1       40s
```

## Creacion de servicio redis maestro
```sh
kubectl apply -f redis-master-service.yaml 
service/redis-master created

kubectl get svc
NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
redis-master   ClusterIP   10.104.206.1   <none>        6379/TCP   54s
```

```sh
kubectl exec -it redis-master-567f9dcd4d-zptsz -- bash 
[ root@redis-master-567f9dcd4d-zptsz:/data ]$ ping redis-master    
PING redis-master.default.svc.cluster.local (10.104.206.1) 56(84) bytes of data.

ó

kubectl exec redis-master-567f9dcd4d-zptsz -- ping -c1 redis-master
PING redis-master.default.svc.cluster.local (10.104.206.1) 56(84) bytes of data.

```

## creacion de redis esclavos
```sh
kubectl apply -f redis-slave.yaml
deployment.apps/redis-slave created

kubectl apply -f redis-slave-service.yaml 
service/redis-slave created
```

## creacion de frontEnd PHP
```sh
kubectl apply -f frontend.yaml                      
deployment.apps/frontend created

kubectl apply -f frontend-service.yaml 
service/frontend created

kubectl get all -l app=guestbook
NAME                            READY   STATUS    RESTARTS   AGE
pod/frontend-74cbf67867-gs28v   1/1     Running   0          70s
pod/frontend-74cbf67867-jftzd   1/1     Running   0          70s
pod/frontend-74cbf67867-zj59t   1/1     Running   0          70s

NAME               TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/frontend   NodePort   10.104.111.46   <none>        80:30531/TCP   65s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/frontend   3/3     3            3           70s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/frontend-74cbf67867   3         3         3       70s
```