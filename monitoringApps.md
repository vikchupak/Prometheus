# Monitoring third-party application

To monitor apps, we need:
- Deploy **Exporter** k8s service for the app
  - Redis exporter: https://github.com/oliver006/redis_exporter
  - Helm Redis exporter: https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-redis-exporter
- Tell Prometeus about the new Exporter
  - Deploy CRD `kind: ServiceMonitor`. It describes a set of targets to be monitored by Prometheus.

List all service monitors
```bash
kubectl get servicemonitor -n monitoring

# alertmanager
# apiserver
# prometheus
# prometheus-operator
# node-exporter
# etc...
```
