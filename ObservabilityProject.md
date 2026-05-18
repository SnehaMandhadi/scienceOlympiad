### Observability Project

link

```
https://youtu.be/w2pnBa7eAbI?si=O6Tta46oyiHrdRG0

```

#### Observability overview

1. observability ahve 2  parts 
    - instrumentation of metrics
    - implementation of observability

2. Instrumentation: your development team who is writign microservices should ommit 
    - metrics
    - logs
    - traces

    ![alt text](image-208.png)


    In below image wee can see metrics are exported
    ![alt text](image-210.png)

3. Implemetation: Devops engineers who are creating kuberentes cluster, who are deploying appciations using helm charts or operators, who are setting up obervability stack like grafana,prometehus,jagger

4. WHy should we use open telemetry / why should developers use opentelmetry and how will it work?

    - if your company whnt to move from Jager to grafana or from datadog to dyanatrace
    - now the developer who is writing microservices is using prometeus client and tomorrow when they want to shift from prmeteus to nagios, then all the 100s of microservices needs this change which you have hardcoded premetheus stack
    - they need to modify how logs are defined, how metrics are defined and how traces are defined
    - so a project called Open telementry was introduced, instead of using prometheus client use opentelemenry
        - API
        - SDK
        - exproter
        In the exproter you can define the client that you want either prmetheus or jagger and open tlementry will take care of it
        - so developers started using opentelementry apis and sdks
        - so in the demo app we are seeing modules and libraries related to Otel
        - it have file=exporter configuration file, where we can define what should be the receiver of htis logs or traces


#### Flow
1. Developers develop microservice, along with applciation code like for golang application, they use

Golang app + open tele api,sdk for momiting metrics,logs,traces and these metrics logs and traces are first received by a receiver component of open telemetry, it will process it using compnent=processor and finally there is an exporter which can export the metrics logs and traces to any backend that you need

- this is defined in the export ocnfg file about your backend = like if I want to use jager for traces

- backen=proemtheus

prometherus= have receiver which received inf from jaeger, it is udpated in TSDB, prometehud have http server which can file prometheus query and get the inf
- prometheus needs expoerters to scrape metrics, the pods used are kube-statemetrics pod,nodexporter pod