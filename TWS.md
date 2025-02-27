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

- Now as we've deployed all apps, we've to monitor all apps.


