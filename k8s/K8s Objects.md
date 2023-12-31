---
cards-deck: k8s
tags: k8s
---

Persistent entities in K8s system, k8s uses them to represent state of the cluster.

`spec` (desired state) & `status` (current state)

Required fields:

- apiVersion
- kind
- metadata
- spec

## K8s Object Management #card

| Management technique      | Operates on          | Recommended environenment |
| ------------------------- | -------------------- | ------------------------- |
| Imperative commands       | Live objects         | Dev projects              |
| Imperative object config  | Individual files     | Prod projects             |
| Declarative object config | Directories of files | Prod projects             |

### Imperative commands #card

```bash
kubectl create deployment nginx --image nginx
```

### Imperative object config #card

```bash
kubectl create -f nginx.yaml
kubectl delete -f nginx.yaml -f redis.yaml
kubectl replace -f nginx.yaml
```

### Declarative object config

```bash
kubectl diff -f configs/
kubectl apply -f configs/
kubectl diff -R -f configs/
kubectl apply -R -f configs/
```

## Object Names and IDs

- DNS Subdomain Names
- RFC 1123 Label Names
- RFC 1035 Label Names
- Path Segment Names

## Labels and Selectors

Examples:
- `"release": "stable"`, `"release": "canary"`
- `"environment": "dev"`, `"environment": "qa"`, `"environment": "production"`
- `"tier": "frontend"`, `"tier": "backend"`, `"tier": "cache"`
- `"track": "daily"`, `"track": "weekly"`

## Syntax and Character Set

## Label Selectors

- equality based
- set based

### equality based

`=`,`==`,`!=`

### set based

`in`, `notin`, `exists` (only the key identifier)
```bash
kubectl get pods -l environment=production,tier=frontend
kubectl get pods -l 'environment in (production),tier in (frontend)'
```

### Resources that support set-based requirements

`Job`, `Deployment`, `ReplicaSet`, `DaemonSet`

### Updating labels

```bash
# labels all NGINX Pods as frontend tier
kubectl label pods -l app=nginx tier=fe
```

## Namespaces
Provides a mechanism for isolating groups of resources within a single cluster.

> for a production cluster, consider not using the `default` namespace.

### When to use multiple namespaces

### Initial namespaces

- `default`
- `kube-node-lease`
- `kube-public`
- `kube-system`

### Working with namespaces

```bash
kubectl get namespace
```

### Setting the namespace for a request

```bash
kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>
kubectl get pods --namespace=<insert-namespace-name-here>
```

### Setting the namespace preference

```bash
# you can permanently save the namespace for all subsequent calls
kubectl config set-context --current \
	--namespace=<insert-namespace-name-here>
kubectl config view --minify | grep namespace:
```

### Namespaces and DNS

`<service-name>.<namespace-name>.svc.cluster.local`

### Not all objects are in namespace

```bash
kubectl api-resources --namespaced=true
kubectl api-resources --namespaced=false
```

## Annotations

## Field selectors

Examples:

- `metadata.name=my-service`
- `metadata.namespace!=default`
- `status.phase=Pending`

```bash
kubectl get pods --field-selector status.phase=Running
```

### Supported fields

They vary by resource type. All resource types support the `metadata.name` and `metadata.namespace` fields.

### Supported operators

`=`,`==`, `!=`

### Chained selectors

```bash
kubectl get pods \
  --field-selector=status.phase!=Running,spec.restartPolicy=Always
```

### Multiple resource types

```bash
kubectl get statefulsets,services --all-namespaces \
  --field-selector metadata.namespace!=default
```

### Finalizers

### Owners and Dependents
