# Prometheus-Grafana

- Prometheus is a metric based monitoring system that helps us ensure that our system and services are healthy and doing well.
- It gives us a whole tool chain of libraries and server components that allow us to expose the metrics we care about. Also helps to collect and record those metrics and do useful things like alerting, creating dashboards, debugging and automation.
- Promethus is written in "GO" programming language and hosted by "Cloud Native Computing Foundation"

System Architecture
-
- Big box in middle is prometheus server. Depending on our need and size of our organization, we can have multiple servers.
- As prometheus is a pull based monitoring system that scrapes metrics over http from a set of systems and services we want to monitor (like app, db, Linux host, sensor in SS). These endpoints are called target. These targets can be our own services or apps that we can directly instrument with metrics (like using client libs) or
- We can list targets to be monitored statically in our prometheus config file or promethus can also talk to wide range of service discovery mechanisms like DNS or K8S API server to auto discover all targets to monitor
- Promethus then pulls metrics from all targets periodically and builds view of metrics over time on local disk on TSTB (Time Series DB). We can use that collected metrics data to generate alerts through "AlertManager" or we can query the data externally either through Web UI, Grafana or PromLens.
- We cal also configure prometheus to forward the samples that it collects to remote storage system

![image](https://github.com/user-attachments/assets/2fe1e645-9d35-47f9-80bd-ebacab70efbf)


