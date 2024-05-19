# minikube

```bash
minikube delete --all
minikube start

eval $(minikube docker-env)

# with appropriate kubectl version
minikube kubectl -- get pods -A

minikube dashboard [--url]

# get service url
minikube service <service-name> --url

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
kubectl explain <resource>

kubectl create deployment hello-node \
	--image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- \
	/agnhost netexec --http-port=8080

# ~/.kube/config
kubectl config view

kubectl delete service hello-node
kubectl delete deployment hello-node
kubectl delete service -l app=<name>
kubectl delete deployments/<name> services/<name>

kubectl proxy

# mk addons enable metrics-server
kubectl top po

kubectl logs <pod-name>
kubectl logs -f <pod-name>
kubectl logs <pod-name> -c <container-name>

# kubectl exec <pod-name> -- <cmd>
# start bash session
kubectl exec -it <pod-name> -- bash
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash

# apply new label
kubectl label pods <pod-name> version=v1

kubectl scale deployments/<name> --replicas=4

# update image
kubectl set image deployments/<name> <image-name>=<image-name>:<version>

kubectl cluster-info

kubectl create configmap <name> --from-literal=key=value
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
kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
kubectl get pods <pod-name> -o jsonpath='{.spec.containers[*].name}'
# kubectl get pods --output=wide
kubectl get pods [-o wide|yaml]
# view the pod and service
kubectl get pod,svc -n kube-system

kubectl port-forward <name> <port>:<port>
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
# For example, we've updated env variable in a Pod via ConfigMap.
# We now have to replace the existing Pod with the new one.
kubectl rollout restart deployments/<name>

kubectl edit svg/<service-name>
kubectl edit configmap <name>
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

## Deploy applications

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
kubectl get services hello-minikube
minikube service hello-minikube
# or
kubectl port-forward service/hello-minikube 8080:8080
```

# K3s

```bash
k3d cluster create -a 2 [--no-lb]
k3d cluster stop
k3d cluster start
k3d cluster delete
k3d cluster create --port 8082:30080@agent:0 -p 8081:80@loadbalancer --agents 2

k3d kubeconfig get k3s-default
```

# Helm

```bash
helm search repo <name>
```
