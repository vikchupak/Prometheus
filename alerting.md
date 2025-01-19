# Alerting

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

# Ways to create Alert Rules

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
  - Prometheus operator takes care of finding that component and applying that rule. And reloading the prometheus.
