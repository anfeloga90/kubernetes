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











```sh

```