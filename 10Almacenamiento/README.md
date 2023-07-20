# Almacenamiento

## comandos

```yalm
apiVersion: v1
kind: Pod
metadata:
  name: volumenes
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /home
      name: home
    - mountPath: /git
      name: git
      readOnly: true
    - mountPath: /temp
      name: temp
  volumes:
  - name: home
    hostPath:
      path: /home/kubernetes/datos
  - name: git
    gitRepo:
      repository: https://github.com/apasoftTraining/cursoKubernetes.git
  - name: temp
    emptyDir: {}
```
Debemos fijarnos que la ruta /home/kubernetes/datos esta previamente creada en el nodo. al subir el pod va a crear las rutas home, git y temp

## Volumenes persistentes
PV = Persistent volume

```yalm
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: volumen-nfs
  nfs:
    path: /var/datos
    server: 192.168.174.131
```
Para usar el volumen debemos crear un PVC



```sh
```

```yalm
```
```sh
```
