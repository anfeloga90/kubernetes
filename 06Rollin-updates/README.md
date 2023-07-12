# Rollin-Updates

## Comandos

```sh

```

## RollingUpdate
los pods se recrean o actualizan de manera pausada

```sh
kubectl rollout history deploy nginx-d
deployment.apps/nginx-d 
REVISION  CHANGE-CAUSE
1         <none>

kubectl rollout history deploy nginx-d --revision=1
deployment.apps/nginx-d with revision #1
Pod Template:
  Labels:       app=nginx
        pod-template-hash=775f464cb7
  Containers:
   nginx:
    Image:      nginx:1.17.8
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>

```

cambiamos la version del nginx del archivo deploy_nginx2.yaml de la version 1.17.8 a la 1.7.9

```sh
kubectl apply -f deploy_nginx2.yaml                
deployment.apps/nginx-d configured

kubectl rollout history deploy nginx-d             
deployment.apps/nginx-d 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>

kubectl rollout history deploy nginx-d --revision=2
deployment.apps/nginx-d with revision #2
Pod Template:
  Labels:       app=nginx
        pod-template-hash=7759cfdc55
  Containers:
   nginx:
    Image:      nginx:1.7.9
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>
```

```yaml
spec:
  selector:   #permite seleccionar un conjunto de objetos que cumplan las condicione
    matchLabels:
      app: nginx
  replicas: 10 # indica al controlador que ejecute 2 pods
  strategy:
     type: RollingUpdate
     rollingUpdate:
      maxUnavailable: 1 # no sera superior a 11
      maxSurge: 1 # no sera inferio a 9
  minReadySeconds: 3 # espera 3 segundos antes de seguir con los siguientes cambios de version
```
## hacer un rolling back
```sh
kubectl rollout history deploy nginx-d
deployment.apps/nginx-d 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
4         <none>

kubectl rollout undo deployment nginx-d --to-revision=2
deployment.apps/nginx-d rolled back

kubectl rollout history deploy nginx-d                
deployment.apps/nginx-d 
REVISION  CHANGE-CAUSE
3         <none>
4         <none>
5         <none>
```

## Recreate
borra todos los pods y luego los crea, por lo que puede ocacionar perdida de servicio de algunos segundos

```sh

```







## 
