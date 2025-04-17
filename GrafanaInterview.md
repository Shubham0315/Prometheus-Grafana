What is Grafana and how is it used in DevOps?
-
- Grafana is open source visualization and analytics platform used to monitor, analyze and explore time series data
- It provides powerful dashboards and graphs for observing infrastructure, app and system performance
  - Grafana helps devops teams to visualize metrics from tools like prometheus where dashboards show CPU usage, disk IO, network traffic, service uptime
  - Provides real time monitoring of apps and infrastructure to identify and resolve issues
  - Allows to setup alerts based on threshold conditions
  - Can also get integrated with CICD, Cloud platforms
 
-----------------------------------------------------------------------------------

How does Grafana differ from other monitoring tools like Kibana or Prometheus?
-
- Grafana is for visualization and dashboarding which focusees on metrics, logs, traces. To monitor dashboards and alerting.
- Prometheus collects metrics and stores them. Based on time series metrics. Used for monitoring metrics
- Kibana is used to explore and visualize log data. Mainly focusses on logs analysis

- Grafana doesnt collect data itself, it connects to various data sources like prometheus and displays data in visual form

-----------------------------------------------------------------------------------

Explain the concept of a dashboard in Grafana. What kind of data can you visualize?
-
- Dashboard in grafana is a collection of panels organized on single screen that visualizes data from one or more sources in real time
- It helps monitor key metrics, trends and system behaviour at a glance
- Its like a control center for our infrastructure, apps, logs and services

- Components in dashboard
  - Panels :- individual visual blocks (graphs, tables, guages)
  - Variables :- Dynamic values that allow you to filter data (hostname, env)
  - Rows/Groups :- helps analyse multiple panels cleanly
  - Time picker :- To select time range
 
-----------------------------------------------------------------------------------

What are panels in Grafana, and how are they used?
-
- Panels are building blocks of Grafana dashboard.
- Each panel is a single visualization that displays data from selected data source

- How panels are used?
  - Data Querying :- Every panel is backed by a query from datas sources like prometheus
  - Data Visualization :- Once queried, data is visualized in line graphs, bar charts, tables, logs, guages, etc
  - Customization :- We can set thresholds and color rules, add panel titles
  - Alerting :- We can set alerts on supported panels
 
-----------------------------------------------------------------------------------

What are the different types of visualizations available in Grafana?
-
- Time series/line graphs :- To plot time based data like CPU, memory, traffic
- Stat :- Displays single numeric value
- Guage :- Circular dial showing value of metric within a range
- Bar chart :- Vertical or horizontal bars for categorical data
- Table :- Great for log summaries, alerts lists, database outputs
- Logs :- Displays log lines
- Pie chart :-
- Histogram :- Shows frequency distribution of values

-----------------------------------------------------------------------------------

What data sources does Grafana support? Can you name a few you've worked with?
-
- For metrics and monitoring we can use prometheus, Graphite
- For logs we can use loki, elastic search
- For cloud monitoring use AWS cloudwatch, datadog, appd

-----------------------------------------------------------------------------------

How do you connect Grafana to Prometheus?
-
- Grafana installed on 3000, prometheus on 9090
- Login to grafana
- Add prometheus as data source - Configure prometheus data source - In HTTP URL http://localhost:9090
- Create first dashboard

-----------------------------------------------------------------------------------

Can you explain how Grafana queries data from a source like Prometheus?
-
- When we create panel in grafana and choose prometheus as data source, grafana sends promQL query to prometheus, receives result, visualizes it acc to selected panel type
- Grafana sends GET request (HTTP API Call) to prometheus
- Prometheus returns JSON data with timestamp, metrics values, labels
- Grafana renders data, parses JSON and displays data using selected visualization panel

-----------------------------------------------------------------------------------

How would you configure Grafana to pull metrics from a cloud provider (e.g., AWS CloudWatch)? 
-
- Prerequisites :- Grafana, AWS account with Cloudwatch metrics, IAM role/user with permissions

- Add AWS creds to grafana to access setting env variable
- Add cloudwatch as data source in Grafana
- Configure cloudwatch source - Provide region, access keys
- Create dashboard panel - Configure namespaces, metric name, statistics like avg/min/max, period

-----------------------------------------------------------------------------------

How does alerting work in Grafana?
-
- Alerting allows to setup notifications for specific conditions or thresholds on our visualized data
- It helps monitor health and performance of system by sending notifications when something goes wrong or when predefined conditions are met

- Key Components
  - Alert rules :- Conditions under which alert is triggered
  - Notification channels :- Methods through which alerts are sent email, SMS
  - Alert state :- nStatus of alert whether its active/resolved or pending
 
- To create alerts
  - Grafana dashboard - select panel - Alert tab
  - Define alert conditions like threshold, query, evaluation interval
  - State alert state OK, Alerting, pending, pauses
  - Configure notification channel
  - Set alert message and frequency of mesaage
 
-----------------------------------------------------------------------------------

What are Alert Rules and Contact Points in Grafana Alerting?
-
- Alert rules define conditions that trigger alert in grafana
- These rules are directky tied to panel in dashboard and are based on metric queries we set in that panel
  - Key components :- Query, threshold, evaluation, alert conditions

- Contact points define where alert notifications are sent once triggered. Channels or endpoints where we'll receive alerts
  - Email, slack, webhook, teams
 
-----------------------------------------------------------------------------------

What are dashboard variables in Grafana, and how are they useful?
-
- They allow to create dynamic, reusable dashboards that can be customized based on user input or context
- They act as placeholders in queries allowing to dynamically filter and modify data presented in dashboard without manually editing queries each time
- It makes easier to handle multiple environments, instances or services in single dashboard

-----------------------------------------------------------------------------------

How do you create a dynamic dashboard using variables and repeat panels?
-
- Creating dynamic dashboard in grafana using variables and repeat panels allows to automatically generate panels based on values of variable
- Super handy to visualize same metric across multiple instances, services without manually duplicating panels

- Create variable - Use variable in panel query - Setup repeat panel
