# Architecture

https://github.com/prometheus/prometheus?tab=readme-ov-file#architecture-overview

- Main Component: **Prometheus server**
  - Does actual monitoring work
  - Made up of 3 parts:
    - **Metrics Storage:** Time Series Database.
      - Stores all metrics data, like CPU usage, Memory usage, etc.
    - **Metrics Retrieval:** Data Retrieval Worker.
      - Pulls metrics data from servers, services, apps. And sends them to Storage DB.
        - Pulls from targets' HTTP endpont `hostaddress/metrics`.
        - So targets have to expose the enpoints and the endpoints have to return data in correct format.
          - **Exporter** component may be needed to fetch metrics from targers and convert them to correct format, and expose them on the Exporter `/metrics` endpont, where the Metrics Retrieval will fetch data from.
            - There are a list of official exporters for many different services like elasticsearch, linux server, cloud platforms, etc.
              - Example: https://prometheus.io/docs/guides/node-exporter/
              - These exporters are available as docker images. So, we can deploy them as **Exporter SIDECAR containers** among with **target containers** inside a pod, and expose `/metrics` for ptometheus to collect the metrics.
            - There are a list of official client libraries for different applications like, node.js, java, etc.
              - Example: https://prometheus.io/docs/instrumenting/clientlibs/
              - Using these libs, we can expose `metrics` endpont
    - **HTTP Server**
      - Accepts PromQL queries
      - API used to display the data in Prometheus Web UI or Grafana

---

- Things being monitored are called **targets**
- Each target **has units(metrics)** being collected, like CPU usage, memory usage, requests count/duration.
- Metrics are human-redable text-based format data.
  - Metrics entries have **TYPE** and **HELP attributes**
    - HELP is a description of what a metric is
    - TYPE is one of 3 metrics types
      - COUNTER (how many times x happened)
      - GAUGE (current value of x now)
      - HISTOGRAM (how long or how big)
      - SUMMARY
 
---

Metrics collecting mechanisms
- **Pull** (Prometheus periodically fetches metrics directly from `/metrics` targets' endpoints)
- **Pushgateway** (To push metrics directly to Prometheus, but this should only be used in exceptional cases, for example for short-lived targets like cron jobs)
- ***Other monitoring systems use*** **Push** (Targets are pushing their matric data themselves to the collection platform [Amazon Cloud Watch as example])
