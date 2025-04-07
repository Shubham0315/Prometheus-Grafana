What is Prometheus? How does it work?
-
- Prometheus is an open source monitoring and alerting toolkit. It is now part of CNCF just like K8S
- Its designed for recording real time metrics in a time series data base with powerful promQL and alerting capabilities

- It is used to
  - Monitor K8S clusters
  - Tracking system health (CPU,memory,disk,network)
  - Observing app performance
  - Triggering alerts based on threshold breaches

--------------------------------------------------------------------------------------

What is a time-series database? How is Prometheus a time-series DB?
-
- TSDB is a type of database optimized for handling time-stamped or time-ordered data. This type of data typically includes measurements or observations tracked, monitored or recorded at specific time intervals
- TSDB Characteristics
  - Time based indexing allowing efficient retrieval based on time ranges
  - Efficient storage and retrieval to store large amounts of time ordered data efficiently
  - High write throughput to involve high frequency data collection
 
- Prometheus stores data as time series meaning each data point is associated with timestamp. This allows prometheus to track how metric changes over time
- Prometheus stores metrics as series of data points in form of key-value pair, which're uniquely identified by metric name and set of labels
  - e.g :- http_requests_total{method="GET", status="200"} 1024
  - This stores total no of GET requests with 200 OK status allowing you to track its value over time.
- Prometheus keeps track of data in chronological order allowing it to perform efficient time based queries
- PromQL is designed for querying time series data which performs complex time-series analysis like computing rates, averages, or aggregating data over different time windows 

--------------------------------------------------------------------------------------

What are exporters in Prometheus?
-
- They help to expose metrics from different systems or services so that prometheus can scrape them and store them as time series data
- Prometheus itself is not aware of every system we want to monitor, so exporters bridge gap by transforming metrics from various services (like DB, web servers, OS) into a format Prometheus can scrape and understand

- Node exporter
  - Monitor system level metrics like CPU, memory, disk I/O, network stats, etc
  - Used to monitor health and performance of Linux based servers
  - Exposes metrics at /metrics on a configured port  (generally 9100)
 
- First install exporter directly on target system or use docker container to run them
- Configure prometheus to scrape the exporter
  - In prometheus.yml file
    -     scrape_configs:
           - job_name: 'node'
             static_configs:
              - targets: ['node-exporter:9100']
  - Start prometheus and visualize with grafana

--------------------------------------------------------------------------------------

What is a metric in Prometheus? Name the types of metrics.
-
- Metric is a measurement of system's state or behaviour, tracked over time. Metrics are typically gathered from various sources like apps, infrastructure, or services and represent aspects like CPU, memory, HTTP request count

- Counter
  - Counter metrics represent monotonically increasing value. It can only increase or be reset at zero (restart)
  - Used to track things like total requests or errors
  -     http_requests_total{method="GET", status="200"} 1024

- Guage
  - It is a metric that represent a value that can go up or down over period of time like temp, memory use, current queue length
  -     memory_usage_bytes{application="myapp"} 204800000

- Histogram
  - It samples a set of values and buckets them into predefined ranges. It also provides count of observations and sum of values
  - We can measure request durations or response sizes where we want to group values into different ranges
  -     http_request_duration_seconds_bucket{le="0.1"} 120
        http_request_duration_seconds_bucket{le="0.5"} 300

- Summary
  - Similar to histogram but it tracks quantiles (%) over time, but also computes total count and sum for given metric
 
--------------------------------------------------------------------------------------
