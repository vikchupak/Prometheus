# Prometheus UI

Very simplistic UI.

Expose Prometheus UI on localhost
```bash
kubectl port-forward service/monitoring-kube-prometheus-prometheus -n monitoring 9090:9090

# http://localhost:9090
```
