# Monitoring third-party application

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
