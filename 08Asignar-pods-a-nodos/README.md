# Asignar pods a nodos

## node selector
Se agregan a travez de las etiquetas

```yalm
spec:
  containers:
   - name: nginx   
     image: apasoft/nginx:v1
  nodeSelector:
    entorno: desarrollo
```

entorno=desarrollo debe estar agregado como label a alguno de los nodos donde solo queremos que se ejecute

deploy_nginx.yaml
```yalm
apiVersion: apps/v1 # i se Usa apps/v1beta2 para versiones anteriores a 1.9.0
kind: Deployment
metadata:
  name: nginx-d
spec:
  selector:   #permite seleccionar un conjunto de objetos que cumplan las condicione
    matchLabels:
      app: nginx
  replicas: 2 # indica al controlador que ejecute 2 pods
  template:   # Plantilla que define los containers
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
