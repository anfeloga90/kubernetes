 # Labels

Sirven para relacionar, localizar y buscar 

tomcat.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
spec:
  containers:
   - name: tomcat
     image: tomcat

```


```sh
kubectl apply -f tomcat.yaml 
pod/tomcat created


kubectl get pod tomcat --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
tomcat   1/1     Running   0          25s   estado=desarrollo
```

### agregar etiqueta
```sh
kubectl label pod tomcat responsable=andres
pod/tomcat labeled

kubectl get pod tomcat --show-labels
NAME     READY   STATUS    RESTARTS   AGE     LABELS
tomcat   1/1     Running   0          6m13s   estado=desarrollo,responsable=andres

kubectl get pod tomcat --show-labels -L estado,responsable
NAME     READY   STATUS    RESTARTS   AGE     ESTADO       RESPONSABLE   LABELS
tomcat   1/1     Running   0          6m37s   desarrollo   andres        estado=desarrollo,responsable=andres

```

### editar etiqueta
```sh
kubectl label pod tomcat --overwrite estado=test
pod/tomcat labeled

kubectl get pod tomcat --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
tomcat   1/1     Running   0          13m   estado=test,responsable=andres
```

### eliminar etiqueta
```sh
kubectl label pod tomcat responsable-
pod/tomcat labeled

kubectl get pod tomcat --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
tomcat   1/1     Running   0          13m   estado=test
```

# Selectores

### buscar
```sh
kubectl apply -f .

kubectl get pods --show-labels
NAME      READY   STATUS    RESTARTS   AGE   LABELS
tomcat    1/1     Running   0          27m   estado=desarrollo,responsable=juan
tomcat1   1/1     Running   0          74s   estado=desarrollo,responsable=juan
tomcat2   1/1     Running   0          74s   estado=testing,responsable=pedro
tomcat3   1/1     Running   0          74s   estado=produccion,responsable=pedro

# Buscar por label
kubectl get pods --show-labels -l estado=desarrollo
NAME      READY   STATUS    RESTARTS   AGE     LABELS
tomcat    1/1     Running   0          28m     estado=desarrollo,responsable=juan
tomcat1   1/1     Running   0          2m15s   estado=desarrollo,responsable=juan

kubectl get pods --show-labels -l estado!=desarrollo
NAME      READY   STATUS    RESTARTS   AGE     LABELS
tomcat2   1/1     Running   0          4m47s   estado=testing,responsable=pedro
tomcat3   1/1     Running   0          4m47s   estado=produccion,responsable=pedro

kubectl get pods --show-labels -l 'estado in (desarrollo,produccion)'
NAME      READY   STATUS    RESTARTS   AGE    LABELS
tomcat    1/1     Running   0          32m    estado=desarrollo,responsable=juan
tomcat1   1/1     Running   0          6m2s   estado=desarrollo,responsable=juan
tomcat3   1/1     Running   0          6m2s   estado=produccion,responsable=pedro
```

### Borrar
```sh
kubectl delete pods -l estado=desarrollo
pod "tomcat" deleted
pod "tomcat1" deleted
```


### Anotaciones
Sirven para describir y documentar nuestros contenedores

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat4
  labels:
    estado: "produccion"
    responsable: "pedro"
  annotations:
    doc: "Se debe compilar con gcc"
    adjunto: "ejemplo de anotacion"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```