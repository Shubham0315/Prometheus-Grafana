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

Prometheus Features
-
- **Data Model**
  - Prometheus tracks and stores time series which're numeric values that change over time and that are sampled at specific points int time.

  ![image](https://github.com/user-attachments/assets/6476251f-be93-4428-90f1-cb02951d30c0)

  - Here question is how do we model a single sample and which values it can take? Also we've to track and query multiple time series at same time so we need way to identify each series and separate them.
  - Every prometheus time series has identifier and ongoing stream of samples that gets build up over time over multiple successive scrapes. Those samples store actual numeric values belonging to a time series
 
  ![image](https://github.com/user-attachments/assets/e9a2a95a-95c3-49e1-a4ad-b5f372ea2817)
  
  - Here each time series is uniquely identified by metric name and set of labels. Metric name is aspect of system or service we're measuring. It could be no of http requests server handled since starting, temparature, memory use.
  - Labels are key value pairs that allow to partition metric name into individual time series so that so as to track in more detail.

  ![image](https://github.com/user-attachments/assets/e976459a-555c-412b-a5f0-988bf6994ccd)

  - Each individual sample consist of timestamp and value. Timestamp is time of scrape and stores as 64 bit integer timestamp in millisecs.
  - Sample value can be any 64 bit floating point number

  ![image](https://github.com/user-attachments/assets/eeefc5bc-3120-4f40-ba2d-e7e355cee9f8)


- **Transfer Format**
  - For prometheus to be able to pull metrics from service, service needs to expose http endpoint which serves text-based metrics transfer format

  - In below SS, each line represent current value of time series along with metrics name and labels
    
  ![image](https://github.com/user-attachments/assets/2ca751c3-629a-40ec-abd5-cc0a625046f2)

  - Text based metrics transfer is easy to generate even without using client libs for tracking and exposing metrics. Also due to pull based, we can navigate to any metrics in browser to view current state of metrics
 
- **Query Language**
  - After collecting metrics we've to do something useful with them using PromQL which allows to query, process collected data for alerting, dashboarding, debugging. We can also perfrom complex arithmetic using it for time series.
  - To check total http requests after starting which is split up by path label, method label and status code

  ![image](https://github.com/user-attachments/assets/4cfe02fd-1c80-466f-bd65-ead3b1bc2e86)

  - Here we can just use metrics name with label needed to tell promQL to select required time series
 
  ![image](https://github.com/user-attachments/assets/658d9279-ce64-4103-a36a-54c68f5aa1bd)

  - To get absolute count from rate of increase, use "rate" function over an averaing time window to see how many requests are happening per second.
 
  ![image](https://github.com/user-attachments/assets/486ae3ff-07e8-4b50-ac23-5cc783d33423)

- **Alerting**
  - To make prometheus detect and alert problems on your infrastructure, we can configure it to load set of alerting rules from files with each rule providing promQL expression and set of metadata fields about the alert.
 
- **Service Discovery**
  - Due to pull based, it knows which targets to monitor, how to fetch metrics from them, attach right labels to them
  - We can manually add targets in config file but it is infeasible today. So prometheus has variety of service discovery mechanisms like DNS, K8S, etc. 
