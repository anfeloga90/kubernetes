# Pods
En pocas palabras, un "pod" en Kubernetes (K8s) es la unidad básica de ejecución. Es un grupo de uno o más contenedores que se ejecutan juntos en un entorno compartido.

Un pod contiene al menos un contenedor, que puede ser una aplicación o un servicio, y también puede incluir contenedores adicionales que brinden servicios complementarios, como almacenamiento compartido, registros o proxies. Estos contenedores dentro del pod comparten recursos y se ejecutan en la misma máquina virtual o física dentro del clúster de Kubernetes.

Los pods son efímeros y se crean y destruyen de manera dinámica en respuesta a las necesidades de escalado, actualizaciones o fallos. Además, cada pod tiene su propia dirección IP y puede comunicarse con otros pods en el clúster a través de la red de Kubernetes.

En resumen, un pod en Kubernetes es un grupo de uno o más contenedores que se ejecutan juntos en un entorno compartido, proporcionando una unidad de ejecución flexible y escalable en el clúster de Kubernetes.
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
kubectl delete pod nginx --now              #Para borrar el pod inmediatamente
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
01-Dockerfile
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
01-nginx.yaml 
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

Crea un pod con dos contenedors, 1 nginx y 1 frontal que le hace ping cada 5 segundos

02-multi.yaml
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
kubectl apply -f 02-multi.yaml 

kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
multi   2/2     Running   0          41s

kubectl logs multi -c frontal
```


## restartPolicy (Always,OnFailure,Never)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: never
  labels:
    app: app1
spec:
  containers:
  - name: never
    image: busybox
    command: ['sh', '-c', 'echo Ejemplo de pod fallado  && exit 1']
  restartPolicy: Always
```