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
```dockerfile
##Descargamos UBUNTU
FROM ubuntu

##Actualizamos el sistema
RUN apt-get update

##En algunas versiones de Linux es necesario configurar una variable para el TIMEZONE
ENV TZ=Europe/Madrid

##Luego creamos un fichero llamado /etc/timezone para configurar 
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

##Instalamos NGINX
RUN apt-get install -y nginx

##Creamos un fichero index.html en el directorio por defecto de nginx
RUN echo 'Ejemplo de POD con KUBERNETES y YAML' > /var/www/html/index.html

##Arrancamos NGINX a través de ENTRYPOINT para que no pueda ser modificado en la creación del contenedor
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]

##Exponemos el Puerto 80
EXPOSE 80
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    zone: prod
    version: v1
spec:
  containers:
   - name: nginx   
     image: apasoft/nginx:v1
```

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


# multi pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi
spec:
  containers:
  - name: web
    image: nginx
    ports:
    - containerPort: 80  
  - name: frontal
    image: alpine
    command: ["watch", "-n5", "ping",  "localhost"]
```





```sh

```