# Redis-chaos

A demo to run chaos experiments on Redis using [Chaos Mesh](https://github.com/chaos-mesh/chaos-mesh)

## Prerequisites

* [Kubernetes](https://kubernetes.io/) version >= 1.12
* [Chaos Mesh](https://chaos-mesh.org/)

## Install Redis

```bash
kubectl apply -f manifests/
```

Check the Redis status:

```bash
kubectl get pod -n default
```

Output:

```bash
NAME               READY   STATUS    RESTARTS   AGE
redis-0            1/1     Running   0          40m
redis-1            1/1     Running   0          39m
redis-sentinel-0   1/1     Running   0          38m
redis-sentinel-1   1/1     Running   0          37m
redis-sentinel-2   1/1     Running   0          37m
```

## Create chaos experiment

You can find some chaos experiment examples in this repo and you can choose one from them randomly to run.

```bash
kubectl apply -f network-latency-slave.yaml
```

This chaos experiment will inject `10ms` network latency between the master node and the slave node. You can use the following commands to check the result:

* Install `ping` command

```bash
kubectl exec redis-0 -- apt-get update
kubectl exec redis-0 -- apt-get install iputils-ping
```

* Check network latency

```bash
kubectl exec redis-0 -- ping -c 2 redis-1.redis.default.svc
```

Output:

```bash
64 bytes from redis-1.redis.default.svc.cluster.local (10.244.3.5): icmp_seq=1 ttl=62 time=10.3 ms
64 bytes from redis-1.redis.default.svc.cluster.local (10.244.3.5): icmp_seq=2 ttl=62 time=10.1 ms
```

## Delete chaos experiment

```bash
kubectl delete -f network-latency-slave.yaml
```

* Check network latency

```bash
kubectl exec redis-0 -- ping -c 2 redis-1.redis.default.svc
```

Output:

```bash
64 bytes from redis-1.redis.default.svc.cluster.local (10.244.3.5): icmp_seq=1 ttl=62 time=0.172 ms
64 bytes from redis-1.redis.default.svc.cluster.local (10.244.3.5): icmp_seq=2 ttl=62 time=0.061 ms
```