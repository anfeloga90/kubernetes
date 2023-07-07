# Minikube
## Comandos
minikube status
minikube logs
minikube ip
minikube profile list

## crear cluster con multiples nodos
minikube start --driver=docker -p cluster-desa --nodes=2

minikube profile list    
```sh                               
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
|   Profile    | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
| cluster-desa | docker    | docker  | 192.168.49.2 | 8443 | v1.26.3 | Running |     2 |        |
|--------------|-----------|---------|--------------|------|---------|---------|-------|--------|
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

