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
