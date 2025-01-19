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
