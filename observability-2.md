### observability proj
```
https://youtu.be/DXZUunEeHqM?si=zRYvMSOmIoT11UzX

Applciation link: https://github.com/LondheShubham153/k8s-kind-voting-app/
```

![alt text](image-225.png)


1. Ec2->ubuntu->t2.medium->keyoair->http->https->storage=15

    ![alt text](image-211.png)

    ![alt text](image-212.png)

    - launch instance

2. observabilty: when we deploy an application in kubernetes then we need 
     - monitoring: CPU,Network,Metrics, are done here= prometheus
     - logging : if login into applcaiton is failed, or errors have occured, then those data are logged
     - tracing
     - alerting : if the load is more then we get alerts using email
     - Visualizing: to see cpu,memory all in dashboard we use Grafana

    ```
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo docker ps = to list running containers on your system
    sudo usermod -aG docker $USER && newgrp docker
    git clone https://github.com/LondheShubham153/k8s-kind-voting-app.git
    ```

3. kind creates a control plane and 2 woorker nodes
4. Need to install kind
    - 
    ```
     chmod +x install_kind.sh
     ./install_kind.sh

     https://github.com/LondheShubham153/k8s-kind-voting-app/blob/main/kind-cluster/commands.md
    ```

     ![alt text](image-226.png)

5. follow the github link to create kind cluster

    ![alt text](image-227.png)

6. to manage this cluster we have to isntall kubectl

    ![alt text](image-228.png)

7. Now I have to install my applciation onto kind cluster. I have 2 ways to do it
    - using argocd
    - or directly deploy manifest of kubernetes

8. I will directly deploy my manifest of kuberentes now I am going into my kubernetes manifest folder

    NOte: our Grafan server runs on port 31000, one of our application voteservice.yml also runs on port 31000, so lets change the applciatoin port to 31002

    ![alt text](image-229.png)

    ![alt text](image-230.png)

    ![alt text](image-231.png)

    we have all the changes so no need to do this

     ```
     kubectl apply -f .
     kubectl get all

     ```
5. Observability: we have to monitor our kubernetes cluster that we have created=kind

    - some applciations are running on this cluster
    - you have to extract metrics
    - we use prometherus to extarct metrics.
    - PRometheus is a timeseries database
    - cpu%=yaxis, time in min=xaxis
    - the cpu % of your cluster is increasing, you get this data to proemtheus and promethius stores this data in timeseries db recordign to its time and we create graphs from data
    - Prometheus have servers 
        - 1. for scraping data
        - 2. for querying
    - we need lot of manifest files to make this application run in kubernets we see below

        ![alt text](image-232.png)

    - similarly for promethus also we need lot of manifest files so if we have all the files packaged into folder that will be easy to handle
    - so we have Helm= it is a package manger where we can install kubernetes manifest files, prometheus,grafana,argo where we can install ,mangae and uninstall in 1 click
    - so Helm is a package manager for kubenetes specific applications
    - let me istall helm before isntalling prometheus

    ![alt text](image-233.png)

6. what are helm charts?
    - all these are packages of manifest files

7. I am creating a repository name prometheuscommunity where all theh health charts of proemtheus and manifest files will be added to it

    ```
    helm repo list

    ```

| Name | Port | type |
|---------|---------|----|
| prometheus | 9090  |  service  | 
| Grafana | 31000 |  service  | 
| Aler manager | 32000 | service  | 
| Nodeexporter | 32001  | service  | 


    ![alt text](image-234.png)

8. what is not exporter= image there is your kubernetes node, then the cpu,memory,nreworkm input,output and all details of htis node get exported to prometheus
 - prometheus have node exporter which collects all data

 9. now I am doing port forwatding for prometehus

    ![alt text](image-235.png)

10. we are exposing sg 9090

    ![alt text](image-236.png)

    Then now give your Ec2 instance ip:9090

    ![alt text](image-237.png)

    you see the port is up and running

    ![alt text](image-238.png)


11. understand this concept

    ![alt text](image-239.png)

    ![alt text](image-240.png)

    ![alt text](image-242.png)

12. Now we have deployed proemtheus in kubernets cluster
13. how do we know if services are connected to prometheus or not
     - go to staus->target health

        ![alt text](image-243.png)

    - we see the alert manager is picking up metrics is up and running

        ![alt text](image-245.png)

    - ipadress:9090/metrics you see all data

        ![alt text](image-246.png)

    - this gives the cpu usage of your; go to graph and give the command to get hte cpu usage in last 15min

        ![alt text](image-247.png)

    ```
    kubectl get svc
    kubcetl port-forward svc/vote 5000:5000 --address=0.0.0.0 &

    ```

    I will expose port 5000 in sg

        ![alt text](image-248.png)

    ecpip:5000 and duplicate instances, so that you are making network usage high

    you can check this by using hte bleow query by removing by pod at end

    ```

    sum(rate(container_network_receive_bytes_total{namespace="default"}[5m])) 
    ```

    ![alt text](image-249.png)


- I am running grafana on 31000

    ![alt text](image-250.png)

    - admin, prom-operator

    ![alt text](image-251.png)
