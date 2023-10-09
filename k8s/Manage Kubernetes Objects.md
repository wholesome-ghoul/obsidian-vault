# Managing K8s Objects Using Imperative Commands

## How to create objects

- `run`
- `expose`
- `autoscale`
- `create`

```bash
# kubectl create <objecttype> [<subtype>] <instancename>
kubectl create service nodeport <service-name>
kubectl create service nodeport -h
```

## How to update objects

- `scale`
- `annotate`
- `label`
- `set`
- `edit`
- `patch`

## How to delete objects

- `delete`

```bash
kubectl delete deployment/nginx
```

## How to view an object

- `get`
- `describe`
- `logs`

## Using `set` commands to modify objects before creation

```bash
kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run=client | \
  kubectl set selector --local -f - 'environment=qa' -o yaml | \
  kubectl create -f -
```

## Using `--edit` to modify objects before creation

```bash
kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run=client > /tmp/srv.yaml
kubectl create --edit -f /tmp/srv.yaml
```

# Imperative Management of K8s Objects Using Config Files

## How to create objects

```bash
kubectl create -f <filename|url>
```

## How to update objects

```bash
kubectl replace -f <filename|url>
```

## How to delete objects

```bash
kubectl delete -f <filename|url>
```

## How to view an object

```bash
kubectl get -f <filename|url> -o yaml
```

## Creating and editing an object from a URL without saving the config

```bash
kubectl create -f <url> --edit
```

# Declarative Management of K8s Objects Using Config Files

## How to create objects

```bash
kubectl apply -f <dir>
# kubectl diff -f https://k8s.io/examples/application/simple_deployment.yaml
kubectl diff -f <filename|url>
```
