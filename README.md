# kubernetes

## comandos

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
kubectl get pod tomcat --show-labels
kubectl get pods --show-labels -l estado=desarrollo
kubectl get pods --show-labels -l estado!=desarrollo
kubectl get pods --show-labels -l 'estado in (desarrollo,produccion)'
kubectl delete pods -l estado=desarrollo

kubectl create deployment apache --image=httpd
kubectl scale deploy nginx-d --replicas=5


```