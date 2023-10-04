1. What is Kubernetes?

Kubernetes is an open source container orchestration tool.

2. How to do maintenance activity on K8s node?
3. How do we control resource usage of the pod?
4. What are the various K8s' services running on nodes and describe the role of each service.
5. What is Pod Distribution Budget?
6. What's the init container and when it can be used?
7. What is the role of Load Balance in K8s?
8. What are the various things that can be done to increase K8s security?
9. How to monitor K8s cluster?
10. How to get the central logs from pod?
11. How to turn the service defined below in the spec into an external one?

```yaml
spec:
  selector:
    app: some-app
  ports:
    - protocol: UDP
      port: 8080
      targetPort: 8080
```
12. Complete the following config spec file to make it ingress.

```yaml
metadata:
  name: someapp-ingress
spec:
```

13. How should TLS be configured with ingress?

```yaml
spec:
 tls:
 - hosts:
   - some_app.com
   secretName: someapp-secret-tls
```

14. Why should namespaces be used? How does using the default namespace cause problems?
15. What service namespace are referred to in the following file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: some-configmap
data:
  some_url: silicon.chip
```

16. What is an Operator?
17. What is the purpose of operators?
18. What is GKE?
19. What is Ingress Default Backend?
20. How to run K8s locally?
21. What is K8s Load Balancing?
22. What the following in the Deployment configuration file mean:

```yaml
spec:
  containers:
    - name: USER_PASSWORD
      valueFrom:
        secretKeyRef:
          name: some-secret
          key: password
```

23. Can you explain the difference between Docker Swarm and K8s?
24. How to troubleshoot if the pod is not getting scheduled?
25. How to run a Pod on a particular node?
26. What are the different ways to provide external network connectivity to K8s?
27. How can we forward the port `8080(container) -> 8080(service) -> 8080(ingress) -> 80(browser)` and how it can be done?
28. What are the main benefits that Deployments offer that Replication Controllers do not?
29. How to validate a cluster created with K8s operations?
