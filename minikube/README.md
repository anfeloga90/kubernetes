# Minikube
## Comandos
```sh
minikube status
minikube logs
minikube ip
minikube profile list
minikube profile cluster-desa
    ✅  minikube profile was successfully set to cluster-desa
```
## crear cluster con multiples nodos

```sh
minikube start --driver=docker -p cluster-desa --nodes=2 --memory 4G --cpus 2
```


```sh
minikube profile list                          
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
|   Profile    | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
| cluster-desa | docker    | docker  | 192.168.49.2 | 8443 | v1.26.3 | Running |     2 |        |
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
```

### actualizar cluster cambiar cpu y memoria
Este cambio hace que se borre el nodo junto con todo lo que tengamos dentro
```sh
minikube config set memory 6G 
❗  These changes will take effect upon a minikube delete and then a minikube start

minikube config set cpus 3
❗  These changes will take effect upon a minikube delete and then a minikube start
```
En este archivo quedan los cambios que acabamos de realizar
```sh
cat .minikube/config/config.json                           
{
    "cpus": "3",
    "memory": "6G"
}                                                                                                                            
```

Para aplicar los cambios de cpu y memoria se debe detener el nodo
```sh
minikube stop
minikube delete
minikube start --driver=docker --nodes=2 --memory 4G --cpus 2
```


## help
minikube provisions and manages local Kubernetes clusters optimized for development workflows.

Basic Commands:
  start            Starts a local Kubernetes cluster
  status           Gets the status of a local Kubernetes cluster
  stop             Stops a running local Kubernetes cluster
  delete           Elimina un cluster de Kubernetes local
  dashboard        Acceder al panel de Kubernetes que corre dentro del cluster minikube
  pause            pause Kubernetes
  unpause          unpause Kubernetes

Images Commands:
  docker-env       Provides instructions to point your terminal's docker-cli to the Docker Engine inside minikube.
(Useful for building docker images directly inside minikube)
  podman-env       Configura un entorno para usar el servicio Podman de minikube
  cache            Manage cache for images
  image            Manage images

Configuration and Management Commands:
  addons           Habilita o deshabilita un complemento de minikube
  config           Modify persistent configuration values
  profile          Obtener o listar los perfiles actuales (clusters)
  update-context   Update kubeconfig in case of an IP or port change

Networking and Connectivity Commands:
  service          Returns a URL to connect to a service
  tunnel           Conectar a los servicios LoadBalancer

Advanced Commands:
  mount            Mounts the specified directory into minikube
  ssh              Log into the minikube environment (for debugging)
  kubectl          Run a kubectl binary matching the cluster version
  node             Usa (add, remove, list) para agregar, eliminar o listar nodos adicionales.
  cp               Copie el fichero dentro de minikube

Troubleshooting Commands:
  ssh-key          Retrieve the ssh identity key path of the specified node
  ssh-host         Retrieve the ssh host key of the specified node
  ip               Retrieves the IP address of the specified node
  logs             Returns logs to debug a local Kubernetes cluster
  update-check     Print current and latest version number
  version          Print the version of minikube
  options          Show a list of global command-line options (applies to all commands).

Other Commands:
  completion       Generate command completion for a shell
  license          Outputs the licenses of dependencies to a directory

Use "minikube <command> --help" for more information about a given command.

