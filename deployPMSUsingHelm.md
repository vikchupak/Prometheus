Just follow doc installation

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

Add helm repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Create separate namespace
```bash
kubectl create namespace monitoring
```

Install helm chart
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

List prometheus pods running
```bash
kubectl --namespace monitoring get pods -l "release=monitoring"
```

List everything created by helm chart in namespace
```bash
kubectl get all -n monitoring 
```

# Description of some conponents created

- `statefulset.apps/prometheus-monitoring-kube-prometheus-prometheus` - actual prometheus server
- `statefulset.apps/alertmanager-monitoring-kube-prometheus-alertmanager` - actual alert manager

---

- `deployment.apps/monitoring-kube-prometheus-operator` - prometheus operator (it created prometheus server and alert manager). Manages/orchestrates the monitoring stack.
- `deployment.apps/monitoring-kube-state-metrics` - installed as nested dependency helm chart. Scrapes **k8s** infrasctucture components out-of-the-box. **So k8s components are monotored out-of-the-box.**

---

- `deamonset.apps/monitoring-prometheus-node-exporter` - connects to server/node and translates the worker node metrics to prometheus metrics. **So Worker nodes are monitored out-of-the-box.**

---

**Also prometeus stack components are also monitored out-of-the-box.**

---

```bash
kubectl get pod -n monitoring
```

- `prometheus-monitoring-kube-prometheus-prometheus-0` - **prometheus pod with 2 containers**
  - `prometheus` - main pod's container. Actual Prometheus container.
  - `config-reloader` - sidecar container that watches for **config changes (like updated alert rules, etc.)** and reloads the prometheus for the updated configs to take effect.
 
- `alertmanager-monitoring-kube-prometheus-alertmanager-0` - **alertmanager pod with 2 containers**
  - `alertmanager` - main pod's container. Actual Alertmanager container.
  - `config-reloader` - sidecar container that watches for **alert manager config changes** and reloads the Alertmanager for the updated configs to take effect.
