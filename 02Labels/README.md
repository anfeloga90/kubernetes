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
k apply -f tomcat.yaml 
pod/tomcat created


k get pod tomcat --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
tomcat   1/1     Running   0          25s   estado=desarrollo
```






### 
```sh

```