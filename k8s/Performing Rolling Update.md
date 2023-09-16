**Rolling Updates** allow **[[Deployment|Deployments]]**' update to be with zero downtime by incrementally updating **[[Pod|Pods]]** instances with new ones.

```bash
kubectl rollout status deployments/<name>
kubectl rollout undo deployments/<name>
```