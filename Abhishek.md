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


