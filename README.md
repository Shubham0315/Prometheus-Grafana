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


Download and Inspect 
-
- After downloading we get below list of files

![image](https://github.com/user-attachments/assets/500c07b8-e660-47d5-8a91-32ae964ed5f4)

- Promtool is a command line tool to check config, running queries or other debugging and testing purposes.
- We only need prometheus config file and server binary. Edit the existing config file and write from scratch
- Here "global" section allows us to set no of global defaults for prometheus behaviour where define default scrape_interval: 5s
- Second section is "scrape_configs" to tell which targets to be monitored, how to discover and scrape them and what labels to apply to each target. Here add scrape config to make prometheus pull metrics from itself
  - Every scrape_config needs to have "job_name"  which sets default value for job label for any pulled time series. Job label is usually used to group together one or more targets of same type (multiple replicated instances of API server). Here in prometheus server there is only one target which will be listening on port 9090, that is TCP scrape address we can configure
  - Endpoint URL :- **http://localhost:9090/metrics**
 
  ![image](https://github.com/user-attachments/assets/f4b11bd2-bf8b-42d7-a358-286e10694648)

- In second scrape_config add 3 instances of demo service that pormLab is running publicly on internet. In prometheus pull-based model we can pull metrics from those demo service instances without having to reconfigure
- Prometheus will load its config from prometheus.yml by default and will store the collected metrics in data/ subdirectory of PWD and will listen on port 9090
- Now we can go to browser and check endpoint URL if prometheus works. The web interface gives bunch of diff status pages that tell us about build and runtime info of server. It also allows to query and visualize collected data using promQL

![image](https://github.com/user-attachments/assets/0dc13c94-3bb5-4bfc-b985-b0ae94ce9433)

- We can also check if the targets are correctly scraped :- Status - Targets
- Here we can see all our targets are up means prometheus has successfully scraped metrics from each of them

![image](https://github.com/user-attachments/assets/ef00e921-19b4-42aa-95fa-82f2389973ea)

- Now go to graph page, where we can enter promQL queries and evaluate them to show results in table or graph over time

![image](https://github.com/user-attachments/assets/3227d5ce-f764-444d-a858-c54034cefa3d)

![image](https://github.com/user-attachments/assets/47a4dad7-b4b7-4db6-a87a-2652962d5601)

- We can also compute the metrics using rate function like below. Use "rate" at beginning and give timeframe at last

![image](https://github.com/user-attachments/assets/913ca0cc-5031-42cf-ae15-dc0720181d13)


Prometheus Metrics Types
-
- There are 4 types :- Guages, Counters, Summaries, Histograms

1. **Guage Metric**
- Guages are just metrics that naturally go up/down as they represent current count of memery, queue length, disk space, temp. Used to measure numeric values and expose them as metric.
- When we add guage to application, instrumentation client libraries give us couple of methods to update value
- Set method :- to set guage for any arbitrary value but there are also methods to increase/decrease value in relative way.

![image](https://github.com/user-attachments/assets/cbc73c3a-0eea-4152-9208-ee1e8f9cd10b)

- In some client libs there're helper methods to store current time as unique timestamp can be used to expose last time some event has happened like boot time, process start time, last run

- In metrics exposition format, guage just shows up single time series like below.

![image](https://github.com/user-attachments/assets/7b58a84c-9980-443f-8883-898cd4fa88d6)

![image](https://github.com/user-attachments/assets/b78c6d88-9866-4302-84a5-f689401e475e)



2. **Counters**
- Represent cumulative count over time like total HTTP requests or total time to handle them.
- Also unlike guages, counters only go up.

![image](https://github.com/user-attachments/assets/2194ff73-92f4-4edb-b7ad-087c10e22832)

- Only exception is when process thats tracking and exposing counter crashes or restarts for some reason. In that cases counter value restarts from 0.

![image](https://github.com/user-attachments/assets/00147390-a6ca-4e82-90be-62b489216340)

- Counter have 2 methods to update value
  - Inc method to increment value by 1
  - For fractional/integer values larger than 1, we can use add() method to increment value by arbitrary amount

![image](https://github.com/user-attachments/assets/c4d93b74-de33-4e6d-a746-736cbf1d548f)

- Counters dont have any methods to increase/decrease values

- In the exposition format, counters look exactly like guages, excpet optional metadata indicates that type of metric is counter


3. **Summaries**
- Summaries are useful if we want to track distribution of request latencies or some other set of numeric values or percentile or

![image](https://github.com/user-attachments/assets/0517d7e0-4679-4ed6-b068-5636de2d9531)

- To create summary in instrumentation we specify which quantiles we want to calculate along with error margins and when we want to track specific value, we call observe() method on summary object with that value

![image](https://github.com/user-attachments/assets/df8569f6-5641-4e54-8627-dffa6119f509)

- If we've handled HTTP request that took 2,3 sec, we can use observe() method with 2.3 to record the duration

![image](https://github.com/user-attachments/assets/ad0e60cc-852c-4cfa-b22c-9e76f5581777)

- In expostion, summaries are expanded into set of time series, one for each computed target quantile and total no/sum of observations
  - Here _count means total requests, _sum means total time to handle requests
 
![image](https://github.com/user-attachments/assets/de3963be-769d-43a1-9449-06b3916a10b4)

- In output of summary, it is collection of guage and counter metrics so in promQL we can use individual series like counters and guages


4. **Histograms**
- Similar to summaries and allow to track distribution of set of numeric values. But instead of computing pre-computed quantiles, histograms counts values in set of ranged buckets to give idea of how many values we've seen for each range.
- Also they are cumulative in nature. Ecah bucket contains count of previous lower-ranged buckets

![image](https://github.com/user-attachments/assets/53d6d16d-5fee-4ddc-8d59-3b15090de4f7)

- In cumulative histogram, we only need to define upper boundary of each bucket

- To create histogram, we've to provide set of bucket ranges to constructor and then observe values like request durations into histogram

![image](https://github.com/user-attachments/assets/fb897a77-00f5-4090-a982-d4cd57ee50ab)

- Histogram auto take care of incrementing right bucket counters

- In exposition, each bucket is exposed as single counter time series with "le" label indicating upper value boundary of that bucket

![image](https://github.com/user-attachments/assets/36159fee-2866-41a2-ad1c-6fa3f07a5fea)

- Buckets should be fine grained but at same time should not be many in number to destroy prometheus server
- Histogram is set of counters and query each one of them individually

- To count approx percentile from histogram :- histogram_quantile() function
- Also as buckets are counters we'll almost want to wrap either rate() or increase() function around histogram bucket before passing them into histogram quantile.

![image](https://github.com/user-attachments/assets/a4fbb960-a8d3-4ed3-9292-866f419f6b79)

- So we can count events in known time range.


Counter rates and Increases in PromQL
-
- When dealing with counter rates, we always want to compute their rate of increase. PromQL gives us 3 diff functions to tell how fast counter is going up.
  - rate(), irate(), increase()
- Counter metrics are usually useless in their raw form. Except for occasional resets to zero when process restarts, they only ever go up
- We need how fast counter has gone up over some defined recent time window. (e.g:- current request rate vc CPU usage)

- rate() :- computes per second rate of increase averaged over the entire input range window.
- irate() :- computes much faster reacting instantaneous rate by only considering last 2 samples under provided window for its rate calculation.
- increase() :- behaves like rate except that it returns absolute value increase over provided time rather than a per second increase

- All the three functions need to find at least 2 samples for eachs eries under provided window to tell how fast counter is going up.

- Imagine we've a counter metric and we've to calculate its rate of increase at single point of time. So for rate of increase fucniton we select five minutes range window

![image](https://github.com/user-attachments/assets/23b0de51-b9ae-4ce1-8e82-1ef2fd7f69b1)

- Question here is how these functions deal with counter resets. To compensate for resets, we first scan entire inout range for samples having lower value than previous and assume any value decrease must have been a reset. Since counters always start from zero after reset, we can assume actual no of increases was at least value of sample before reset plus lower subsequent value. So we can add values of the reset to previous ones

![image](https://github.com/user-attachments/assets/3c3f292d-5cbe-42f7-a796-ec6d262051e6)

