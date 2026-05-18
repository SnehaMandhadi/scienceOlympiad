### OBservability abhihsek

1. consider a Nurse checking patient healt for every hour and if doctor want to understand the health o fpatient he will look at the inf doctor will decide if health is stable or after medication if heart beat is going down then doctor decides patient needs to be critically observved so using metrics doctor can understand health of patient
2. Metrics: historical data of events to understand the health of system. Using metrics you can understand helth of sytem
    - drqw back: data is raw and if it too much of numerical data
3. if you feed this data to a monitoring sytem and represents metrics in  a dashboard or graphical format then if will be easy to understand 

    ![alt text](image-252.png)

4. The heartbeat is collected every 15min either by nurse or by using monitoring system. Monitoring system puls the metrics and represent in the dashboard in from of pie chart or graph
5. In monitoring syterm you can also setup alert
    - if herartbeat of patient goes over 110 send wats app msg to doctor to immediatley attend patient

6. Monitoring system is capable of reading metrics, represent them in dashboard and send alerts when required
    ![alt text](image-253.png)

7. there is a  applciation that is deployed in the AWS or on k8s, to understand the state of entire system like not only app,about k8s, about infrastructure, so if any of this have prob your app wil not work properly so to monitor the sytem we need metrics
    - eg: CPU of vm on AWS or k8 nodes like at 10am what is cpu utilization of nodes
    - eg: memory of your nodes
    - eg: pod status: At what time pod went into crash loop back off, or deployment status, replicas
    - application specific metrics: total no of http req recieved in a day
    - how many users have signed out in last 30days how much time users are spending on platform

8. Monitoring sytem: we can pull(scrape) or push the metrics from monitoring system and represent in graph format
    - egL stock market: at a particurlat time this is the rate of stock

9. Monitoring systtem can fire alerts:
    - if a cpu utilization goes above 80% send alert
    - if disk usage is greeater than 75% send slack message
    - users are seeing a latency- http req  processing are taking 10sec  happening 30times in a day then fire alert

    ![alt text](image-254.png)

10. Prometheus: it will pull metrics from various sources, represents in dashboard format, in prometheus we have alert manager to fire alerts
    - node exporters
    - kube state metrics
    - alert manager

    - retrival: pull metrics from exporters or  from push gateway, and stores that info in timeseries db
    - Timeseries db : stores data based on time 
    - supports promQl: when you pass  promQl query to http servier using user interface, promethus will give data
    - We have 100s of app in my kubernetes cluster, how can prometheus identify which app metrics it has to be pulled? using service discovery we can give targets 

    - Default components of prometheius server:

        - Retrival
        - TSDB
        - HTTP Server
    - we need to install alert manager in proemetus server

11. PRoemteus is intergrated with Grafan and both of them together is used for strong monitoring stack

we need kuberentes cluster= use minkube,or EKS or kind cluster

12. create EKS cluster
    - create cluster
    - associate oidc provider: when your prometheius with in eks want to communicate with any aws resources then we need this
    - create node group

13. ing rafana dashboard->connections->add new connection->add prometheius url
14. How does preometehues scrape lot of metrics?
    - prometheus use exporters to scrape all metrics
    - eg:node exporter it is default one it will be installed

        ![alt text](image-280.png)

    - it can get the node related metrics, it goes to each VM or node of your cluster, from proc files it can get cpu utilization every min and stores these metrics
    - prometehus will pull these metrics
    - Another exproter= kube state metrics= this will get all inf from kubernetes api server= pod status, deployment status,config maps,secrets and proemtheus will pull metrics from kubernetes
    - it is a default exporter

        ![alt text](image-282.png)

    - we have 2 instances of node exporter but only 1 instace of kubestate metrics? why= because node exporter runs as daemonset= it has to collect inf from each and every node individually
    kubestate metrics= will call talk to api server of kubernetes cluster it is not bothered about which node it is(refer Deployment_daemonSet_statefulset.md)
15. Prometehus:
 - ***node exporter***= gets all data  related to nodes/Virtual machines like cpu usage= it will collects infrastaructure level metrics
    - how node expoeter get data?= it will communicate with virtual machines or nodes and reads the system files of nodes
 - ***kube state metrics***= gets all data related to resources on kubernets clusters like pods,deployments,configmap,scerets
    - how kubestate metrics get this info: it talks to the api server
- ***/metrics endpoint***:  APplication level metrics=http reqeusts data, or user level metrics: the develpers of app has to write the metrics api (/metrics endpoint)

16. we have copetitors of premethues like nagios,influxdb,graphite
    ![alt text](image-281.png)

17. using promql we can speak to one of the component of prometheus server= HTTPserver and using this server we can fetch data that is stored in TSDB

18. I want to get the ipadress of service
    ![alt text](image-291.png)

   -  service type=clusterIp it is very good practice, port of node-exporter=9100
   - clusterIp =cannot be accessed on your browser (pods_service.md)
   - we will take out aws account, ec2 instances=nodes of kubernetes cluster, take 1 ec2 instance= connect= curl < IPaddress of your node exporter> < port of nodeexporter>/metrics
   - ![alt text](image-299.png)

   - that is the inf which your node exporter is collecting periodically
   - node exporter will store all data at < IPaddress of your node exporter> < port of nodeexporter>/metrics  endpoint

   - ![alt text](image-300.png)

   - this info is understandable by prometehus
   - do the same for kube-metricsip adress port /metrics to get kubernetes data 
   - eg here you will be having all the container data

![alt text](image-301.png)
 these data have lables

 ![alt text](image-302.png)

- now I will go to prometheus dashboard and take this name 

![alt text](image-303.png)

 - the above command container restart total= is telling how many times the container is restarting
    
19. we are writing a pod that will always crash
    ```
     kubectl run busybox-crash --image=busybox -- /bin/sh -c "exit 1"
    ```
    ![alt text](image-304.png)

    ![alt text](image-305.png)

    ![alt text](image-306.png)

    - how many times pod have crashed
    - how many times config maps have created
    - how many secrets are there in kubernetes cluster
    - cpu,memory,disk utilization,http requests,users created

    - Prometheus does basic things= setup alerts,metrics collection,visualization =vam

20. Grafana: it is visualization dashboard: total pids, created, toatl secrets created
 - you can do authentication and authorization
 - integrate IAM
 - my management can only view, devops team can edit

    ![alt text](image-313.png)

    ![alt text](image-314.png)
    - crash loop backoff= intiaally it will crash in 1 min, then 3 min,then 5 min,..

21. types of custom metrics:
    - guage
    - caunters
    - summary
    - historgram

    service monitor:

    - if we have lot of services that developers have endpoints
22. Instrumentation: 
    - if developers are not omitting the metrics,logs and defining traces then the complete monitoring stack like prometheus,grafana is of no use
    - expoerters= it will get info related to node=node exporter, if you want metrics related to your application you need to have instrumentattion
    - similar to data types in programming lang we have  metrics types in instrumetation
    - type of metrics:
        - counter
        - guage
        - histogram
        - summary

        - counter: the metrics which will be always increasing.
             eg: creating account logins
             - the number of http requests received in your applciaton
        - Guage : this can be incremetning as well as decrementing
            - eg: CPU utilizagiton,memory utilization

        - Histogram: not bothered about increasing or incremetning and decrimetning then we create buckets
         eg: http req duration: how many times the http req have taken less than 5 ms

        ![alt text](image-325.png)

        - we need lables, metric type, name

        - lets deploy aour applicatiom
        - ![alt text](image-326.png)
        - 
        - ![alt text](image-327.png)

        - ![alt text](image-328.png)

        - these are all the endpoints it supports
        - ![alt text](image-329.png)
        - we need to make sure the custome health check that we wrote is proemtheus able to read those

        - This is one of the custom metrics that we can test

        - ![alt text](image-330.png)
        - ![alt text](image-331.png)

        - we need to do service discovery then only prometheus will know that from checih app does it have to fetch data from
        -  you need to deploy a yaml file and that yaml file will take care of service discovery
        - ![alt text](image-332.png)
        - match service a at /mettis endpoint

        - ![alt text](image-333.png)

        - ![alt text](image-334.png)

23. How to filre alerts:
    - we need to configure which alerts to you want to fire in alertmanager config file
    - ![alt text](image-335.png)
    - I want alerts for high CPU usage and pod restart
    - we shall intentially crash our pod and check if alert triggers

    - ![alt text](image-336.png)
    - google account->manage google account->security-> app passwords
    - we need to enable 2 factor authentication on our email
    - name: alert manager
    - ![alt text](image-337.png)
    - ![alt text](image-338.png)
    - ![alt text](image-339.png)
    - ![alt text](image-340.png)
    - ![alt text](image-341.png)
    - update emailid 
    
    ```
     kubectl apply -k .
     kubectl get pods -n dev
    ```

    - now go the application and use the endpoint called /crash
    - ![alt text](image-342.png)

24.  why do we need logging?
    - if the end users want to understand what applciation was doing at a particular time, then  we need to have logs. Developer will be writting logs in order to triage the issues facing when running an applciaton
    - helps to debug the application

25. EFK stack: 
    - if you have 100s of micreoservices in your orgnisation  that are runnign in differenct namespaces
    - image if you have db issue or log4j issue amongest one of hte applciation
    - how do you kow which applciation is having this issue interemittetnly
    - you can go to each and every pod and check the logs of each pod and identify the app having issue
    - or you can create a centrlized logging system wher all the applciaton service logs are colleted in 1 place and her you can excute teh query either through UI interface or theough CLI and you can serach for DB connection message
    - iif there are security related issues you have less tiem to react. so if you have cetnralized logs it is easy to react fast
    - Elastic search
    - fluent bit: similar to proemtheus which ahve TSDB, prometheus uses node expoerter,kube state metrics to retrive metrics from applciatons, there is a graphical repsentatiuon and dashboard in grafana. simalarly fluent bit which is deployed as demon set exactly like node exporter(deployed on each node of kubenets cluster), fluent bit also deployed as damonset on each node and reads logs from pods of kubernetes cluster at a particular location and forwards these logs to elastic serach which itself is a db.
    - the logs are stored in elastic serach. you can  attach elastic search to EBS vaolumes, and take bakc up of logs on timely basis and can create snapshots
    - There is a visuablization dashboard similar to grafana called kibana

    - ![alt text](image-343.png)

    - I haveflunet bit whihc is forwarding logs to elastic search, we will use Elastic search as stateful set and we will mount it as Ebs volume

    - My elastic search is on Kubernetes cluster where as EBS is outside the EKS cluster. ofcoaurse both Elastic search and EBS volumes are in AWS environmet and one is in EKS and other is EBS, so volumes are Ebs volumes, how does elastic serach in EKS cluster communicate with EBS volumes?

    - we use IAM role where kubernets Service account of Elastic serach is mapped to IMA role and this IAM role can talk to the EBS volume

    - Elastic stack cannot direeectly cconneect to EBS voluse we need CSI EBS driver along with IAM role
    - csi EBS driver will help to create storage class and persistent volume

26. EKS cluster:
     - create EKS cluster
     - create service account using csi driver
     - refer this to know about csi errors:
        https://github.com/SnehaMandhadi/AbhishekNotes/blob/main/3-tierArchitecture.md

    -  ![alt text](image-344.png)
    - CSI driver is required because , in EKS when you use PVC, you need a driver that can create EBS volume this is wher you create CSI driver

    - 1. create name space=logging
    - 2. deploy component by compnent first add repo= elastic search
        ![alt text](image-345.png)
    - 3. Helm install elastic search
    -   ![alt text](image-346.png)
    - 4. need to get username and password for elastic searech
    - 5. If flent bit(flow=passing loggs to elastic search) want to forward loggs to elastic serach we need username and pwd of elastic serach

        ![alt text](image-347.png)

    - 6. Lets install Kibana, we are installing kibana as load balancer service type

        ![alt text](image-348.png)
    - 7. upddate eelastic search values in fluent bit values.yml

        ![alt text](image-350.png)

    - 8. install fluent bit

        ![alt text](image-351.png)

    - 9. Fluent bit got depoyed as pod
        - ![alt text](image-352.png)

        - Elastic search is runninga s sttaeful set
        - it is a 2 node lubectl cluster so fluent bit will run as demonset(so we see 2 nodes=because it havd 2 podes)

            ![alt text](image-353.png)

        - kibana as load balancer service type

             ![alt text](image-354.png)
        - kibana port=5601

            - loadbalancer adress:5601
            - username=elastic

            ![alt text](image-355.png)

            - this kibana dashboard is autmatically connected to elastic search and fluentbit is forwardign logs to elastic search
        - we donot have any applciations running on our eks cluster so we are not seeing any logs

        - ![alt text](image-356.png)
        - fluent bit= logging namespace
        - applciation=service A= dev namespace
        - we are checking if fluent bit is able to collect loggs of service A

        - ![alt text](image-357.png)

        - we are able to see the service b logs in fluentbit, it has forwarded that to elastic search

        - go to elastic srach and create view

        - ![alt text](image-358.png)
        - ![alt text](image-359.png)
        - ![alt text](image-361.png)

        - save dataview for kibana
        - ![alt text](image-362.png)
        - we can see all logs with time stamps

        - FLuentbit->elasticsearch-kibana
        - Fluent bit= vendor neutral

    10. fluent bit components:
         - service
         - input: it is coming from conatiner logs
         - output: we will define where fluent bit have to forward logs, in our case we are forwardign to elastic search.
         - if you are running fluent bit on AWS EKS cluster, we need to make sure that TLS is on 

            ![alt text](image-363.png)
         - filters: we can ingnore particular namespaces
27. Traces:
    - At whhat time you got issue, we can check this using traces
    - why it took so long?= this can be done using traces
    - what arre thhe different services involved in the path
    - all this is possible only if we implement tracign in our applications
    - 2 parts to implement tracing:
        - developers have to do instrumentation
        - using open Telemtry
            ![alt text](image-364.png)
        - 1. we can define tracing at fucntion level, or at function level= developers will take care
        - 2. implementing tracing: Developers/SRE will do this
        - Tool: Jaeger:
28. Components of Jagger:
    - Agent(Tracer)= that gets traces from applciation. only if developers have isntrumented traccing in application
    - collector(collects info with it)
    - DB(Elastic search)/cassandra= we need to configrure
    - UI: we fire query to collector and we will get data

    ![alt text](image-365.png)

    - Grafana,kibana,jager - URLs share with them
    - for taking snapshots of db and storing them we use EBS volumes using CSI Driver
    - EBS have persistent volumes

29. EKS cluster, create service account +CSI driver
- IAMrole= if 1 service in AWS want to talk to other service in AWS
- now Elastic serch is deployed as pod inside EKS, so we need to create a service account for Elastic search, and tie this service account with IAM role= using OIDC connector
- elastic search is usingg EBSS volume

    ![alt text](image-366.png)

- get the ARN of serivce account, create addon,create namespace
- helm repo add ,install
- we need to provide elasti serach username and password + CA certificate in Jaeger
- for providing certificate in Jaeger= we can use config map or secrets, for providing certificates we can use configmaps
- helm repo add, install Jaeger
- we need toupdate jager values about elastic serach details, port forwarding

    ![alt text](image-367.png)
    - jager UI is sending req= but pod is down= crash loop backoff

    - ![alt text](image-368.png)

    ```
    kubectl describe pod podname -n namespace

    ```

    - ![alt text](image-369.png)

    -  ![alt text](image-370.png)

    - Jaeger liveliness probe, readingess probe have failed, that is why crash loop backoff is coming= issue wih configuration= thee pods are refering to old references

    - ![alt text](image-371.png)

    - it took 6 spans= 6 points
    - traces are divided into spans

    - ![alt text](image-372.png)
    - what issue did you solve using tracing?
        - there was an issue whre service a  and servic eb communication is taking lot of time
        - after implemneted tracing using jaeger,I was able to see why it is taking more time and shared info to developer, devoper could see info and  able to figure out why it having latency and so we devops team with developer team was able to fix the issue



Summary: For customer metics
- instrument metics
- setup premtehus
- service discovery

Observabu=ility:
 - Metrics- instrumentaiton,implementation: helps tp understand what is the state of system=prometheus,grafana
 - Logs: why is the applcaiton wrong= EFK stack
 - traces - instrumentation,implementation : how ot fix the issue=jagger