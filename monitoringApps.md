# Monitoring third-party application (like `redis`)

To monitor apps, we need:
- Deploy **Exporter** k8s service for the app
  - Redis exporter: https://github.com/oliver006/redis_exporter
  - Helm Redis exporter: https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-redis-exporter
- Tell Prometeus about the new Exporter
  - Deploy CRD `kind: ServiceMonitor`. It describes a set of targets to be monitored by Prometheus.

Just follow the doc
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
redis-values.yaml file for helm
```yaml
serviceMonitor:
  enabled: true
  labels:
    release: monitor # By this label prometheus discovers the component

redisAddress: redis://<redis-service-name>:<redis-service-port> # Tells to the exporter where the redis is
```
```bash
helm install redis-exporter prometheus-community/prometheus-redis-exporter -f redis-values.yaml
```

List all service monitors
```bash
kubectl get servicemonitor -n monitoring

# redis-exporter
# alertmanager
# apiserver
# prometheus
# prometheus-operator
# node-exporter
# etc...
```

- **Redis-exporter is deployed as a separate pod, not sidecar container**
- But, it could be deployed as a sidecar container as well

So, `http://<redis-service-ip>:<redis-service-port>/metrics` endpoint to be scraped by prometheus

```bash
kubectl get pod

# redis-pod
# redis-exporter-pod
```

# Monitor own/custom applications (like `node.js server`)

- Client libraries are used for this.
- The Libs implement the prometheus metric types
- No separate exporter. `/metrics` endpoint is exposed by the app itself.
