# What?

I need a cluster of (generally 2) redis instances with sentinel failover.  Masters must persist across restarts, which means that redis needs to be able to write to its config file and the config needs to remain stable.

# How?

```
kubectl apply -f redis-configmap.yaml redis-sentinel-configmap.yaml
kubectl apply -f redis-services.yaml
kubectl apply -f redis-statefulset.yaml
kubectl apply -f redis-sentinel-statefulset.yaml
```
