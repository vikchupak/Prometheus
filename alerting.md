# Alerting

https://samber.github.io/awesome-prometheus-alerts/

- Define what we want to be notified about (Alert Rules in Prometheus Server)
- Send actual notifications (Alertmanager)

---

- We can see alert rules in Prometheus UI
- Firing an alert means sending the alert to `Alertmanager`
- In `expr` we define conditions, func can be used
  - https://prometheus.io/docs/prometheus/latest/querying/functions/#functions
- `labels` is used to group rules
- `for` is used to wait before sending notification to check if the issue resoles itself.
- `anotations` is used to as descriptions of what has happened

## Ways to create Prometheus Alert Rules

- **Without Prometheus operator.**
  - Find `rules_files` file location. Add a rule we want to apply.
    - **Steps to Identify the Rules File Location**
      - Open the Prometheus Configuration `prometheus.yml` file.
      - Look for the `rule_files` section to find the path(s) of your rules files.
       ```yaml
       rule_files:
         - "/etc/prometheus/rules.yml"
         - "/etc/prometheus/alerting-rules/*.yml"
       ```
  - Reload Prometheus app for the changes to take effect.

- **Using Prometheus operator.**
  - Create/deploy custom k8s component defined by CRD(Custom Resource Definition) `kind: PrometheusRule`.
    - Official doc https://docs.openshift.com/container-platform/4.17/rest_api/monitoring_apis/prometheusrule-monitoring-coreos-com-v1.html
  - Prometheus operator takes care of finding that component **by matching labels** and applying that rule. And reloading the prometheus.

  ### Example: Alerting and Recording Rules
  
  prometheus-rules.yaml
  ```yaml
  apiVersion: monitoring.coreos.com/v1
  kind: PrometheusRule
  metadata:
    name: example-prometheus-rules
    namespace: monitoring
  spec:
    groups:
    - name: example-alerts
      rules:
      - alert: HighCPUUsage
        expr: avg(rate(process_cpu_seconds_total[5m])) > 0.8
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage detected"
          description: "The average CPU usage is above 80% for the last 2 minutes. Current value: {{ $value }}"
      - alert: InstanceDown
        expr: up == 0
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Instance down"
          description: "The instance {{ $labels.instance }} is down for more than 5 minutes."
  
    - name: example-recording-rules
      rules:
      - record: job:http_requests_per_second
        expr: sum(rate(http_requests_total[1m])) by (job)
  ```
  
  ```bash
  kubectl apply -f prometheus-rules.yaml
  ```

# Configure Alertmanager

Alertmanager is a separate app. Like Prometheus server. So, this is why **it has its own configuration file**.


- It has simplistic UI.
  Expose Alertmanager UI to host machine
  ```bash
  kubectl port-forward svc/monitoring-kube-prometheus-alertmanager -n monitoring 9093:9093

  # http://localhost:9093
  ```
- When prometheus detects rule in **firing** state it sends the alert to Alertmanager.
- Alert manger then sends actual notification via email, slack, etc.

## Ways to configure Alertmanager

- **Without Prometheus operator.**
  - Find `/etc/alertmanager/alertmanager.yml` file location. Adjust the config.
  - Restart Alertmanager for the changes to take effect.
  
- **Using Prometheus operator.**
  - When deployed via Prometheus operator, the Alertmanager config file is stored in **secret** k8s component.
  - Create/deploy custom k8s component defined by CRD(Custom Resource Definition) `kind: AlertmanagerConfig`.
    - Official doc https://docs.openshift.com/container-platform/4.17/rest_api/monitoring_apis/alertmanagerconfig-monitoring-coreos-com-v1beta1.html
  - Prometheus operator takes care of finding that component **by matching namespace** and applying that config. And reloading the Alertmanager.

  ### Example `AlertmanagerConfig` YAML
  ```yaml
  apiVersion: monitoring.coreos.com/v1alpha1
  kind: AlertmanagerConfig
  metadata:
    name: example-alertmanager-config
    namespace: monitoring
  spec:
    route:
      groupBy: ['alertname', 'cluster']
      groupWait: 30s
      groupInterval: 5m
      repeatInterval: 1h
      receiver: email-receiver
    receivers:
    - name: email-receiver
      emailConfigs:
      - to: team@example.com
        from: alertmanager@example.com
        smarthost: smtp.example.com:587
        authUsername: alertmanager@example.com
        authPassword:
          name: alertmanager-secret
          key: email-password
    inhibitRules:
    - sourceMatch:
        severity: critical
      targetMatch:
        severity: warning
      equal: ['alertname', 'cluster']
  ```
  
  ```bash
  kubectl apply -f alertmanager-config.yaml
  ```

## Useful

See all received alerts by Alertmanager
```
http://localhost:9093/api/v2/alerts
```
