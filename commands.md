## Get help on commands
kubectl `<command>` -h

## Get explanation for resource fields
kubectl explain RESOURCE [options]

e.g.: kubectl explain deployment.spec.template

## Define helpers
export now="--force --grace-period 0"

export do="--dry-run=client -o yaml"

## Delete pods without waiting
kubectl delete pod NAME $now

## Create resource definitions and pipe to file

### ConfigMap
kubectl create configmap NAME [--from-file=[key=]source]
[--from-literal=key1=value1] [--dry-run=server|client|none] [options]

e.g.: kubectl create configmap sample-cm $do > sample-cm.yaml

### Deployment
kubectl create deployment NAME --image=IMAGE -- [COMMAND] [args...] [options]

e.g.: kubectl create deployment NAME --image=image $do > my-deployment.yaml

### Pod
kubectl run NAME --image=image [--env="key=value"] [--port=port]
[--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND]
[args...] [options]

e.g.: kubectl run nginx --image=nginx $do > my-pod.yaml

### Secret
kubectl create secret [flags] [options]

e.g. kubectl create secret generic NAME --from-literal=password=very_complex $do > literal.yaml

## Get a Shell to a Running Container
[Documentation](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)

kubectl exec -i -t NAME --container CONTAINER_NAME -- sh -c "clear; (bash || ash || sh)"

## kubectl Cheat Sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/