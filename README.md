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
kubectl get quota

kubectl create deployment apache --image=httpd
kubectl scale deploy nginx-d --replicas=5

kubectl get endpoints
kubectl expose deploy apache1 --port=80 --type=NodePort
# namespace por defecto
kubectl config set-context --current --namespace=n1

kubectl get events -n n1
kubectl get events -n n1 --field-selector type="Warning"

#Rolling update
kubectl rollout history deploy nginx-d
kubectl rollout history deploy nginx-d --revision=1
kubectl rollout status deployment nginx-d
kubectl rollout undo deployment nginx-d --to-revision=2

# configmap
k create configmap datos-mysql-env --from-env-file datos_mysql.properties 

# instalar metrics server
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
helm upgrade --install metrics-server metrics-server/metrics-server
# "Failed to scrape node" err="Get \"https://192.168.65.4:10250/metrics/resource\": x509: cannot validate certificate for 192.168.65.4 because it doesn't contain any IP SANs" node="docker-desktop"
# para solucionarlo debemos editar el deployment y agregarle la siguiente linea --kubelet-insecure-tls
```

```yalm
      containers:
        - name: metrics-server
          image: registry.k8s.io/metrics-server/metrics-server:v0.6.3
          args:
            - '--kubelet-insecure-tls'
            - '--secure-port=10250'
            - '--cert-dir=/tmp'
            - >-
              --kubelet-preferred-address-types=InternalDNS,InternalIP,ExternalDNS,ExternalIP,Hostname
            - '--kubelet-use-node-status-port'
            - '--metric-resolution=30s'
```

```sh

```