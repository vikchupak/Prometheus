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

- Dashboard is a set of one or more **panels**
- Dashboards are organized in folders
- You can create your own dashboards
- Row is a logical divider within a dashboard
- Rows are used to group panels together

---

- Panel is a basic visualization building block
- Composed by query and visualization
- Each panel has a **query editor** spesific to the data source selected in the panel
- Can be moved and resized within a dashboard

---

Structure:
- Folders
- Dashboards
- Rows
- Panels

---

- **When select edit panel, we can see promQL queries used for visualization. And we can edit these queries.**
- In `Connections` setting, we can configure sources to visualize data from, like prometheus.
