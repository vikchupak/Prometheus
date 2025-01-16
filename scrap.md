# Scraping

- Scraping is proccess of pulling targers' data.
- Targets to be scraped are defined in `prometheus.yml`
- `Service discovery` mechanism is used to find targets' endpoints

---

- How often to scrap `scrape_interval`
- Scraping rules (checks conditions) `evaluation_interval`
- What to scrap `targets`

Prometheus can scrap itself.

# Alert manager

Is used to send alerts when rules conditions are met.

# Data storage

Prometheus data are stored on build-in local time series database. But Prometheus can integrate with remote storage systems.

# PrompQL Query Language

Used to get data or visualize in grafana

# Scaling Prometheus | Prometheus Federation

Prometheus server to scrape data from other Prometheus servers.

# Prometheus with docker and k8s

- Prometheus components are available as docker images and can be deployed in k8s.
- Once deployed in k8s, then it monotors nodes resources out-of-the-box without any extra configuration.
