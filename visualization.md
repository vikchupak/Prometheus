# Prometheus UI

Very simplistic UI. But it contains low level data.

`job_name` is used to group the same type instanses metrics by `job` label.

Expose Prometheus UI to host machine
```bash
kubectl port-forward service/monitoring-kube-prometheus-prometheus -n monitoring 9090:9090

# http://localhost:9090
```

# Grafana UI

Expose Grafana UI to host machine
```bash
kubectl port-forward service/monitoring-grafana -n monitoring 8080:80

# http://localhost:8080

# login: admin
# password: prom-operator
```
