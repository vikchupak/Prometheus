# Prometheus UI

Very simplistic UI. But it contains low level data.

`job_name` is used to group the same type instanses metrics by `job` label.

Expose Prometheus UI on localhost
```bash
kubectl port-forward service/monitoring-kube-prometheus-prometheus -n monitoring 9090:9090

# http://localhost:9090
```
