Why Monitoring?
-
- If in our organization we've 1 K8S cluster, we can monitor it on personal level. But if it is used by multiple teams and someone is facing issue like service not accessible. If we have multiple K8S cluster for dev, staging, prod we'd need monitoring platform like prometheus.

- Grafana is used to visualize data

Prometheus Architecture
-
- Prometheus server has HTTP server which collects all info from K8S cluster. As K8S has inbuilt API server which exposes lot of metrics. Prometheus stores all info in TSDB (STores info of metrics of K8S cluster)
- It also has monitoring system like alert manager which we can configure and send notifications to diff platforms. Promethuse push alerts to alert manager and we configure it to send notifications
- We can also go to prom UI and execute promQL queries. 

![image](https://github.com/user-attachments/assets/5f978fe1-59a1-417c-a76a-19d75afd7cbc)

Grafana
-
- For data visualization. We get data in output running promQL in JSON format which we can represnet in charts using grafana retrieving info from prom.

Demo
-
- Use minikube cluster :- **minikube start**

![image](https://github.com/user-attachments/assets/a9d452b4-61eb-4142-9990-31ed70c9ef80)

- If we check pods, we can see it has all default installations like API server, control manager, etcd and others.

![image](https://github.com/user-attachments/assets/fa9ef274-b24a-4c30-822f-b541b56c52bc)

- To install prometheus using helm or operators
  - Add the helm repo chart and update it
 
![image](https://github.com/user-attachments/assets/c19ee2af-b1fc-46d9-b356-bdd4e5bdf6f6)

  - Now install prom controller

![image](https://github.com/user-attachments/assets/fb5393a9-beda-4040-849a-71f3c226d2a1)

  - Check if prom is installed

![image](https://github.com/user-attachments/assets/f4d1a200-f979-43ba-b1d0-4f78e37d3480)

  - K8S API server exposes few metrics of K8S like API, default installatioms. But we might need more infor from K8S like pods, servers, deployments. For that kube-state-metrics give us a lot of info about K8S cluster

![image](https://github.com/user-attachments/assets/2d44b31b-6429-46a5-b08a-cfbc8f541654)

  - Check the k8s svc's if they are running . Now to expose our prometheus server and see how API looks like. So convert the SVC's clusterIP mode to Node-port mode

![image](https://github.com/user-attachments/assets/3402941e-e890-46c0-b632-afe1be74e07a)
![image](https://github.com/user-attachments/assets/8bd35295-3459-471a-96f6-b9363e1bb391)

  - Now if we check svc we get new entry of exposed service

![image](https://github.com/user-attachments/assets/ba25797a-1d0a-4ca9-a66e-22daf3c5f609)

  - Get the IP address of minikube cluster (command:- minikube ip) and open new tab if prom is running using the port

![image](https://github.com/user-attachments/assets/35147fe2-3064-428e-83ad-895cb857c807)

- To add grafana helm charts

![image](https://github.com/user-attachments/assets/33f3c6cd-9c74-4a3d-b1fa-c59369be02fd)

- To install grafana on K8S minikube cluster :- **helm install grafana grafana/grafana**

- To visualize info from prometheus on grafana dashboard, we need password to login to grafana

![image](https://github.com/user-attachments/assets/57a6f991-2bdc-4f58-9e82-7289902836f1)
![image](https://github.com/user-attachments/assets/f2486903-d883-490e-a5ee-d7d92e112972)

- To expose grafana svc onto node-port mode

![image](https://github.com/user-attachments/assets/2c2b3ded-9d93-4da1-8bb4-034d3da69749)

- Expose using publicIP:31281, login using user and passcode
 
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

- Now we need to create prometheus as data source for grafana as grafana is for visualization so it needs metrics to create dashboards
  - Create data source - prometheus - provide IP address of prometheus - Save and test
 
  ![image](https://github.com/user-attachments/assets/11cc6dd7-1fed-467c-8773-15c846d457eb)
  ![image](https://github.com/user-attachments/assets/e0e8872d-da3a-4e34-9789-550fc5c64abf)
  ![image](https://github.com/user-attachments/assets/c9ad068b-93ec-405e-8371-f31acd723d6a)

  - Now grafana will be able to retrieve info from prometheus, use prom as DS
 
- Now to create dashboard

![image](https://github.com/user-attachments/assets/aa250f09-852d-4e1e-be68-ea128bb09256)

  - We can import dashboard as well (described in TWS)
  - For starting import - use ID as 3662 and create
  - This dashboard retrieves info from our minikube cluster

![image](https://github.com/user-attachments/assets/469d62e6-8a0d-402f-b67e-39d042d14f56)

  
- While executing queries in prometheus, we get info in JSON format

- To know deployment status, running replicas, status of svc, we use kube-state-metrics

![image](https://github.com/user-attachments/assets/df1ed72f-6575-4bdb-984b-65cd25a04394)

- Run the kube-state metrics in browser now, we can see metrics of things in K8S cluster
  - We can take queries from there and execute on prometheus, we get info
 
  ![image](https://github.com/user-attachments/assets/32938f91-0ff0-45e4-8f65-3d56ef8073b8)
  ![image](https://github.com/user-attachments/assets/1f6559f2-2ddc-4b4d-9acc-d99348b38bb6)

  - Grafana takes same info and gives o/p in visualization pattern
  - Using grafana we check K8s info like node uptime, status of k8s api server, status of etcd
 
  ![image](https://github.com/user-attachments/assets/948f2f91-aa05-4f16-a6af-65ed09f82bf1)

