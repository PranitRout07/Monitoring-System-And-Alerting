# Monitoring System Health: Detecting and Alerting on High CPU Usage Thresholds

### Tools used : 
-----------------
##### 1.Node Exporter : A tool used for exposing system metrics at port 9100 . 
##### 2.Prometheus : Monitor the time series data .
##### 3.Grafana : A tool which helps in visualizing the metrics .
##### 4. Docker : It helps in encapsulating the application and its dependencies into compact units called docker containers . Here i have used docker to run Node Exporter , Prometheus and Grafana .

# Steps To Do The Entire Setup
##### 1. Run a Node Exporter docker container . 
```
docker run -d -p 9100:9100 --name=node_exporter prom/node-exporter
```
![node-exporter](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/647e1411-b2ab-43c2-a69b-b269b2d35a3e)


##### 2. Run Prometheus docker container . 
```
docker run -d -p 9090:9090 --name prometheus prom/prometheus
```

![prometheus](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/de32615f-7eac-40d8-99c4-e944b794f720)

##### 3. Run Grafana docker container .
```
docker run -d -p 3000:3000 --name grafana grafana/grafana
```
![grafana](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/aecaf1f6-1f48-4c5f-aabf-e04c329b19d7)


##### 4. See the system metrics at port http://localhost:9100/metrics/

![metrics](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/4ef82727-4aeb-45f6-b3fd-90f6e55b9f93)


##### 5. Go inside prometheus container . 
```
docker exec -it prometheus sh
```

![Inside Prometheus Container](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/1cecb74e-072d-484f-bf13-fa1195ff4f86)


##### 6. Edit the prometheus.yml . Here add the endpoint from which prometheus can scrape the system metrics . 

```
- job_name: "node_exporter"
  static_configs:
    - targets: ["172.30.57.94:9100"]
```

![edit prometheus yaml file](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/d8a8df8a-ca30-4cc9-a271-24b87805bd62)

##### 7. Restart the prometheus docker container .

```
docker restart prometheus
```

![restart the prometheus container](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/0064a0d5-7aea-40f5-9ffa-16bcecdcd761)

##### 8. Verify whether the endpoints are healthy or not on the prometheus dashboard .

![verify whether all the endpoints are successfully connected with prometheus or not](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/e6a81ca9-3ef2-41f5-a5c4-c9779d8221ec)

##### 9. Now enter into grafana container . (here root access is required)

```
docker exec -it -u 0 grafana sh
```

![enter into grafana container](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/6d33a6f5-084f-442d-8944-0c17351bf8be)

##### 10. Edit default.ini . Here basically configure SMTP part . 

![edit defaults ini](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/ecbc41fd-cc22-4bc9-ae78-3d29888b83a4)

##### 11. Restart Grafana container .
```
docker restart grafana
```
![restart grafana server](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/60dbbc37-353d-42c5-844c-665ee8d0303e)

##### 12. Now on the grafana , go to alerting . Here create a contact point by giving the user email . 
![create a contact point using your email](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/fa4b524d-4cd1-4477-b0f7-31cab54bb12e)


##### 13. Now test the contact point . Here check whether the email send is successful or not ?


![test the contact point](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/c8df6843-4846-4585-9e52-54b7c27557e3)


##### 14. Now add this new contact point to default notification policy . 

![edit notification policy](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/e7d4da84-ff5d-4dae-859c-05f07e759e24)


##### 15. Integrate prometheus and grafana . On grafana , use prometheus as a data source .

![use prometheus as data source in grafana](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/c50e0ac9-495b-4504-bd47-add056676eaa)

##### 16. Create dashboards . Here i have created dashboards for system cpu usage and memory usage . 

![create dashboards](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/0e0ce4e7-8d53-4fbf-8b4b-1147d5c65bc9)

##### 17. For a particular dashboard panel create alert rule . Here i have created alert for CPU usage . 
![setup alert rule](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/156aacfe-45fa-401c-822e-1c316de4ff24)


##### 18. Now increasing by system cpu usage using stress . (For testing purpose)

```
stress --cpu 4 --timeout 300s

```


![increasing my cpu usage](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/d9bd83f7-2bcc-4389-8853-bd16f0b70d73)


##### 19. After sometime we can see alert notification is firing from grafana . 

![alert is firing](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/ad781625-eff2-4ea8-817d-b3c322e61fb1)


##### 20. Alert notification received on my email.

![received alert notification on email](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/671cf886-2f8c-4723-873d-a36fec96e31c)


##### 21. Then interrupted the stress command .

![stopped stressing mu cpu](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/20434079-468b-4494-89df-2a85faff83aa)


##### 22. After sometime of interrupting the stress command , CPU usage will go down and we can see it in grafana dashboard . 

![now cpu is correctly utilizing](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/10e6525f-66ca-4f08-97c6-9040def1b04e)


##### 23. Resolved CPU Usage alert notification is received on my email . 


![resolved](https://github.com/PranitRout07/Monitoring-System-And-Alerting/assets/102309095/c260ade4-3b8f-4f41-8e1d-a6ad060af9fe)
























































































































