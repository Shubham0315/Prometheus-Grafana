Make one instance named "Observability-demo" on Linux distro with "t2.medium" instance type. Allow HTTP/HTTPS port, take 15GB storage

Observability
-
- There are 4 imp things about observability :- Monitoring apps after deployed, collect logs using logging, tracing, alerting
- Suppose we've deployed app. Its CPU, networks, metrics we need to know which can be done by monitoring
- If our sign in gets failed or service giving error, error logs gets collected -- Logging
- If we encounter any error but we've to get to that error, we can use tracing to calculate.
- If our CPU usage goes beyond some threshold, we get email using alerting, that workload is increasing

- All these things when put into visualizations, dashboards are generated where we can check CPU, memory, network. Thay are our metrics which we can see graphically

![image](https://github.com/user-attachments/assets/56219bda-4cd0-4e54-b626-a60c9a0c18e4)

- For monitoring, metrics tracing we use prometheus. For visualize things we use Grafana

Practical
-
- Connect the instance created through SSH

![image](https://github.com/user-attachments/assets/44da1f2e-800f-4a47-99d8-2c975f8e0c1f)

- To setup our system for observability, K8S we will need to install some things.  First update the system (sudo apt-get update)
- Now install docker :- sudo apt-get install docker.io -y
- Docker is used as our cluster will be made which will be of kind K8S cluster (KIND means K in D). This we can run on our local as well

![image](https://github.com/user-attachments/assets/a362af11-4ec0-46d1-b91b-bf29b4a16d61)

- Append the group docker with current logged in user and refresh thre group

- Clone the repository in youtube video

- Go inside voting app. Lets say we have config file in which we have one control plane and 2 worker nodes to be made

![image](https://github.com/user-attachments/assets/334700df-4757-473a-8763-3aad1261353e)

- Here, kind will create docker container of control plane and worker nodes. We need to install kind also

![image](https://github.com/user-attachments/assets/d4f3fd05-c46c-4fe8-9327-5ac7401caaa2)
![image](https://github.com/user-attachments/assets/18883ab5-cd7a-4ac1-872d-3d46fa88eb34)

- Now we can make cluster from kind. 
  - Command :- **kind create cluster --config=config.yml --name=my-cluster**

![image](https://github.com/user-attachments/assets/47b2c211-289e-4d51-833d-51c5ff35354a)

- Now to manage the cluster we need to install kubectl. We can see control plane and 2 worker nodes ready

![image](https://github.com/user-attachments/assets/20e3c919-c243-4093-b513-cc4bb71ba8dd)
![image](https://github.com/user-attachments/assets/cee4eac4-53b7-4b9a-b2cc-f2c4b1551327)

- Now to deploy our app :- **kubectl apply -f .**
- Check deployment, pods, services using :- **kubectl get all**

![image](https://github.com/user-attachments/assets/8f8a55d3-e34a-43c9-ac07-15bc0d3b4554)
![image](https://github.com/user-attachments/assets/014449d9-de91-4295-b6f0-ac03c2b4467d)

- Now as we've deployed all apps on K8S, we've to monitor all apps. So we've to put metrics for apps using prometheus

- Prometheus is a time-series database. In our K8S cluster suppose we make graph of data time vs CPU and provide to prometheus which stores that data in TSDB using which we can make graphs for visual understanding
- Prometheus basically has 2 servers :- one for scraping (to extract data from cluster) and other for query (for the data extracted)

- Now we've app running in K8S cluster. We need multiple manifest files to make prometheus. If we package those files, to install, uninstall, manage becomes easier
- So here K8S package manager can be used :- HELM. It helps install prometheus, grafana using one click. It is package manager for K8S manifest files using which we can manage resources on K8S.

- So to install prometheus we need to install HELM first.

- getHelm.sh - shell script will be created which has all commands to install heml

![image](https://github.com/user-attachments/assets/36b72c56-47e0-4913-be4c-f434278601cf)

- The manifest files inside helm are called "Helm Charts"

- Using below command we make prometheus community named repo where all the helm charts of prometheus get added in it.

![image](https://github.com/user-attachments/assets/728362c2-5b72-465f-9d7b-e22e5e7eb4e1)

- Now install one stable chart as well

![image](https://github.com/user-attachments/assets/1f547b3f-d860-479d-b9de-27d9d2525974)

- Now update the repo :- helm repo update

- Now create a K8S namespace "monitoring" inside which we'll be having our prometheus and grafana installed. :- kubernetes create namespace monitoring

- Now we'll provide 30000 as node port to prometheus, set service type as node-port, give 31000 port to grafana, grafana service to be node-port again, alert manager at 32000 with node-port, also made node-exporter using which we feed data to prometheus using port 32001. Using this command helm will install everything for us

- Command :- **helm install kind-prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --set prometheus.service.nodePort=30000 --set prometheus.service.type=NodePort --set grafana.service.nodePort=31000 --set grafana.service.type=NodePort --set alertmanager.service.nodePort=32000 --set alertmanager.service.type=NodePort --set prometheus-node-exporter.service.nodePort=32001 --set prometheus-node-exporter.service.type=NodePort**

![image](https://github.com/user-attachments/assets/b7db6f24-98d4-4be4-bacc-45999c241c7c)

- We can get list of pods in created namespace "monitoring" using command :- **kubectl get pods -n monitoring**

![image](https://github.com/user-attachments/assets/433377a8-5bd1-4a74-a614-f06dee23fb18)

- For K8S node's CPU, memory, network, etc details are exported using node exporter and given to prometheus.

- To get services :- **kubectl get svc -n monitoring**

![image](https://github.com/user-attachments/assets/b91958b8-7b84-4c7f-bb04-85e8fe53e437)

- To forward the port means if we forward using CLI and then go inside our EC2 - security - expose port 9090, we can see our prometheus server

![image](https://github.com/user-attachments/assets/05aac733-8574-4906-88e3-3434985f608b)
![image](https://github.com/user-attachments/assets/dde0c581-3387-40c6-b2da-276faa625ae1)

- Now open new tab and take publicCIP of instance with port 9090, we can see prometheus

![image](https://github.com/user-attachments/assets/62d7bb93-2656-4e41-9a8a-6e6d5ed4cf96)

- To check services are connected, K8S cluster data, go to status - targets
  - here we can see prom alert manager extracting the data, API server everything is visualized in metrics
 
![image](https://github.com/user-attachments/assets/70df0ada-a625-4ab5-a8be-b7bf1bc437fb)

- Go to graphs. inside expression we write promQL.
  - If we've to check container CPU usage in default K8S namespace and get its rate :- **sum (rate (container_cpu_usage_seconds_total{namespace="default"}[1m])) / sum (machine_cpu_cores) * 100**
  - This means calculate rate of container cpu usage seconds. If we execute the query and go to graphs, we get below as CPU usage adjusting time range.  

![image](https://github.com/user-attachments/assets/9ae570b3-08a1-4a1b-85b3-5c3b2ccc125d)

  - To check network in pod :- sum(rate(container_network_receive_bytes_total{namespace="default"}[5m])) by (pod)

![image](https://github.com/user-attachments/assets/9760a455-9586-44c8-83a9-17a8e85f56ef)

- Now lets expose our ""vote" app :- kubectl port-forward svc/vote 5000:5000 --address=0.0.0.0 &. Also add its security groups in EC2
  - Now if we use the public IP and with port 5000, we can see our app is running. So now if we check our CPU, we can see CPU is increased
 
![image](https://github.com/user-attachments/assets/5cbc9895-08db-47f4-9da0-bb356e2fbdf0)

Grafana 
-
- Used to make visualized dashboards
- We already have our grafana installed earlier with helm on port 80 and mapped with 31000
- Command :- **kubectl port-forward svc/kind-prometheus-grafana -n monitoring 3000:80 --address=0.0.0.0 &**

![image](https://github.com/user-attachments/assets/505af71d-0e30-4055-8e34-a512ff2163c2)

- Expose the port 3000 in SG of EC2 for grafana. We can see below UI

![image](https://github.com/user-attachments/assets/0a6ef73d-0473-4a2c-a677-1a008dc410aa)

- Grafana is used to add data source and make dashboards. From DS we get data and goes to grafana. Dashboards will make better visualization of data
- We can make dashboards, set alerts, see connections, do administration

![image](https://github.com/user-attachments/assets/a907c357-81c3-434f-92a1-71c7992df07d)

- In administration - Create users.
- If we dont want to give access to our dashboards, give only view access

![image](https://github.com/user-attachments/assets/f90f40a9-5c54-4c24-b7c6-586f88e9e024)

- To setup connections in grafana. As we have DS and dashboards here. We already have DS added here as we've deployed prom and grafana through helm

![image](https://github.com/user-attachments/assets/e9cc99e0-35a4-4a28-bd24-e10309488463)

- Now to make dashboard using these DS - Build dashboard - Add visualization

![image](https://github.com/user-attachments/assets/6ab45922-dd48-4262-baf7-3e9c1d6f009d)

- Now select the DS to add. Select metric, add labels and run query to get data 

![image](https://github.com/user-attachments/assets/ead2a7d0-d47e-4b66-b3e6-61a29e72aa69)

- On the right top we can see suggestions for dashboard types and apply as required

![image](https://github.com/user-attachments/assets/187dd2da-e6c4-4235-b0d9-441d2bb0503c)

- We can also add other visualizations as needed

- Now to add K8S dashboards- Go to grafana dashboards on google and search K8S there - Add dashboard of choice - Click
  - To import it on our grafana - Copy ID to clipboard - Go to grafana server - Dashboards - New -Import 
 
![image](https://github.com/user-attachments/assets/a2585aa7-018e-4775-8bd8-73561193f1f4)
![image](https://github.com/user-attachments/assets/35f9382b-3ce5-4f0e-975b-8152cc1a77ea)
![image](https://github.com/user-attachments/assets/e72a7de0-3d97-4db2-ba40-bbd3ba80bd23)

By this way we've setup prometheus and grafana

