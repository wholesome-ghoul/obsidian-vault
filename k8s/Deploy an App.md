**[[Deployment|Deployments]]** instruct how to create and update **instances** of app.

**[[Control Plane]]** then schedules app instances on nodes.

self-healing

common:

```bash
kubectl <action> resource

kubectl get [deployments | pods | events | nodes | services | rs] \
	[-o wide] \
	[-l app=<label>]
```