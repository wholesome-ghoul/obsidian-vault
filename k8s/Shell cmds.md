# minikube

```bash
minikube start

minikube kubectl -- get pods -A

minikube dashboard [--url]

# automatically open service in browser
minikube service hello-node

minikube addons list
minikube addons enable metrics-server
minikube addons disable metrics-server

minikube stop

minikube ip
```

# kubectl

```bash
kubectl create deployment hello-node \
	--image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- \
	/agnhost netexec --http-port=8080

kubectl config view

kubectl delete service hello-node
kubectl delete deployment hello-node
kubectl delete service -l app=<name>
kubectl delete deployments/<name> services/<name>

kubectl proxy

kubectl logs <pod-name>

# kubectl exec <pod-name> -- <cmd>
# start bash session
kubectl exec -it <pod-name> -- bash

# apply new label
kubectl label pods <pod-name> version=v1

kubectl scale deployments/<name> --replicas=4

# update image
kubectl set image deployments/<name> <image-name>=<image-name>:<version>
```

## get resource

```bash
# kubectl <action> <resource> [specific-name]
# kubectl describe <resource>
# kubectl get <resource>
kubectl get replicasets
kubectl get deployments
kubectl get events
kubectl get nodes
kubectl get services
kubectl get rs
kubectl get pods -l app=<label>
# kubectl get pods --output=wide
kubectl get pods [-o wide]
# view the pod and service
kubectl get pod,svc -n kube-system
```

## service

```bash
# Expose the Pod as a K8s Service to access from outside of K8s virtual network
kubectl expose deployment hello-node \
	--type=LoadBalancer --port=8080
```
## rollout

```bash
# confirm update
kubectl rollout status deployments/<name>
kubectl rollout undo deployments/<name>
```

## Exposing an External IP Address to Access an Application in a Cluster

```bash
kubectl apply -f <filename>
kubectl get deployments <name>
kubectl describe deployments <name>
kubectl get replicasets
kubectl describe replicasets
kubectl expose deployment <name> --type=LoadBalancer --name=<service-name>
kubectl get services <service-name>
kubectl describe services <service-name>
kubectl get pods --output=wide
kubectl delete services <service-name>
kubectl delete deployment <name>
```
