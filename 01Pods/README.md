# Pods

## Comandos

```sh
kubectl run nginx1 --image=nginx
kubectl get pods
kubectl get pods -o wide
kubectl describe pod nginx1
kubectl describe pod/nginx1
kubectl exec nginx1 -- ls
kubectl exec nginx1 -it -- bash
kubectl logs nginx1
kubectl logs nginx1 -f --tail=20
kubectl port-forwarding nginx1 9999:80
```

```sh
kubectl expose pod nginx1 --port=80 --name nginx1-svc --type=NodePort

kubectl get svc                 
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        60d
nginx1-svc   NodePort    10.100.11.234   <none>        80:31195/TCP   35s

curl localhost:31195
```

## 01 nginx
```sh
docker build -t apasoft/nginx:v1 . -f 01-Dockerfile


docker images | grep nginx
REPOSITORY               TAG           IMAGE ID       CREATED          SIZE
apasoft/nginx            v1            a3953fb126e9   12 seconds ago   163MB

#ejecutamos el docker
docker run --rm -d -p 80:80 --name nginx apasoft/nginx:v1

curl localhost:80
Ejemplo de POD con KUBERNETES y YAML

docker stop nginx

#ahora en k8s
kubectl apply -f 01-nginx.yaml 

```

Crear servicio
```sh
kubectl expose pod nginx --name=nginx-svc --port=80 --type=NodePort

k get svc              
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-svc    NodePort    10.96.150.85   <none>        80:30878/TCP   2m44s

curl localhost:30878
Ejemplo de POD con KUBERNETES y YAML

```



  





```sh

```