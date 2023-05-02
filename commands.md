## Get help on commands
kubectl `<command>` -h

## Get explanation for resource fields

```
kubectl explain RESOURCE [options]
e.g.: kubectl explain deployment.spec.template
```


## Define helpers
```
export now="--force --grace-period 0"

export do="--dry-run=client -o yaml"
```
## Delete pods without waiting
```
kubectl delete pod NAME $now
```

## Create resource definitions and pipe to file

### ConfigMap
```
kubectl create configmap NAME [--from-file=[key=]source]
[--from-literal=key1=value1] [--dry-run=server|client|none] [options]

e.g.: kubectl create configmap sample-cm $do > sample-cm.yaml
```

### Deployment
```
kubectl create deployment NAME --image=IMAGE -- [COMMAND] [args...] [options]

e.g.: kubectl create deployment NAME --image=image $do > my-deployment.yaml
```

### Ingress
```
kubectl create ingress NAME --rule=host/path=service:port[,tls[=secret]]  [options]

e.g. kubectl create ingress example-ingress --rule=hello-world.info/=web:8080 $do > hello-world-ingress.yaml
```

### Pod
```
kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...] [options]

e.g.: kubectl run nginx --image=nginx $do > my-pod.yaml
```

### Role
```
kubectl create role NAME --verb=verb --resource=resource.group/subresource [--resource-name=resourcename] [--dry-run=server|client|none] [options]

e.g. kubectl create role testadmin --verb='*' --resource='*' $do > my-role.yaml
```

### RoleBinding
```
kubectl create rolebinding NAME --clusterrole=NAME|--role=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none] [options]

e.g. ubectl create rolebinding testadminbinding --role=testadmin --serviceaccount=test1:myaccount $do > tastadmin-role-binding.yaml
```

### Secret
```
kubectl create secret [flags] [options]

e.g. kubectl create secret generic NAME --from-literal=password=very_complex $do > literal.yaml
```

### Service
```
kubectl create service [flags] [options]

e.g. kubectl create service TYPE NAME --tcp=80 $do > my-svc.yaml

or e.g. kubectl expose deployment NAME --type=NodePort --port=9000
```

## Get a Shell to a Running Container
[Documentation](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)

```
kubectl exec --it POD_NAME --container CONTAINER_NAME -- sh
```

or

```
kubectl exec -i -t POD_NAME --container CONTAINER_NAME -- sh -c "clear; (bash || ash || sh)"
```

### Interactive Throw-Away Pod for e.g. Debugging
kubectl run -it --image nicolaka/netshoot dns-test --restart=Never --rm

## kubectl Cheat Sheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

## Taints/Tolerations, nodeSelector, nodeName

### Taint
```
kubectl taint nodes NODE_NAME KEY_1=VAL_1:TAINT_EFFECT_1 ... KEY_N=VAL_N:TAINT_EFFECT_N [options]
```

### Tolerations
Pod specification:
```yaml
tolerations:
- key: "key1"
  # operator: "Equal" # set only if `value` not specified
  value: "value1"
  effect: "NoSchedule"
```

> effect: `NoSchedule` | `PreferNoSchedule` | `NoExecute`

### nodeSelector
Select node by label in pod specification:

```yaml
spec:
	containers:
	- ...
	nodeSelector:
		<LabelKey>: <LabelValue>
```

### nodeName
Select node by name in pod specification:

```yaml
spec:
	containers:
	- ...
	nodeName: <NodeName>
```

## Scale Deployment
```
kubectl scale --replicas=<REPLICAS> deployment/<DEPLOYMENT_NAME> 
```

## Rollback Deployment
Check rollout status: `kubectl rollout status deployment/<DEPLOYMENT_NAME>`

Check rollout history: `kubectl rollout history deployment/<DEPLOYMENT_NAME>`

Check revision details: `kubectl rollout history deployment/<DEPLOYMENT_NAME>` --revison=<NUMBER>

Rollback to previous revision: `kubectl rollout undo deployment/<DEPLOYMENT_NAME>`

Rollback to specific revision: `kubectl rollout undo deployment/<DEPLOYMENT_NAME> --to-revision=<NUMBER>`

## crictl

https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/

Used to debug container runtimes on a node.