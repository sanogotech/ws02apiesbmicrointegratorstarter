# ws02apiesbmicrointegratorstarter
API  ESB  Micro Integrator ESB  API Manager

## Stack WS02

The Technology Stack Organizations that do implement microservices often adopt the MSA style along with most of the complementary technologies listed below. | | | |--|--| 

**|WSO2 Technology|

Ballerina
Microgateway
Micro Integrator
API Manager
Enterprise Integrator
Identity Server| |IDE|Eclipse
IntelliJ
VSCode| |Code Repository|Github
Gitlab| |Binary Repository|Maven
Ballerina Central
JFrog
|CI/CD|Jenkins
Travis
Codefresh| |Test Frameworks|SOAP UI
JMeter
TestNG| |Configuration Management Tools|Puppet
Chef
Ansible
|Container Orchestration|Kubernetes
OpenShift
Mesosphere
DC/OS
Hashicorp
Nomad
Docker Swarm |Service Mesh|Istio
Linkerd
Conduit
nginMesh| |Observability|Monitoring

**Application Metrics**

WSO2 Analytics Server (WSO2 API Analytics, WSO2 Integrator Analytics, etc.)
Prometheus/Grafana
Splunk
AppDynamics

**System Metrics**
Icinga
AWS Cloud Watch

**Logging**

Fluentd
ELK Stack
Beats - Agents that ship data to Logstash or Elasticsearch. Filebeat will ship the WSO2 logs to Logstash.
Logstash - Processes and structures the log files received from Filebeat and sends to Elasticsearch.
Elasticsearch - Stores and indexes the logs received by Logstash.
Kibana - Visualizes the data stored in Elasticsearch

**Distributed Tracing**
Jaeger
Zipkin
AppDash
AppDynamics
WSO2 Analytics Server (WSO2 API Analytics, WSO2 Integrator Analytics, etc.)

##  Docs
- https://github.com/wso2/docker-apim
- https://wso2.com/integration/install/docker-compose/community/get-started/
- https://wso2.com/integration/integration-studio/
- https://medium.com/think-integration/building-a-crud-restful-api-with-wso2-micro-integrator-d8637a8cb709

## API  Manager

* https://github.com/wso2/docker-apim

```
docker-compose up --build
```

Access the WSO2 API Manager web UIs using the below URLs via a web browser.
```
https://localhost:9443/publisher
https://localhost:9443/devportal
https://localhost:9443/admin
https://localhost:9443/carbon
```
Login to the web UIs using following credentials.

```
Username: admin
Password: admin
```

Please note that API Gateway will be available on following ports.
```
https://localhost:8243
https://localhost:8280
```

## Entreprise Integrator

- https://wso2.com/integration/install/docker-compose/community/get-started/
```
Recommended system requirements:
3 GHz Dual-core Xeon/Opteron (or latest)
8 GB RAM
10 GB free disk space
```

![EI Components](https://github.com/sanogotech/ws02apiesbmicrointegratorstarter/blob/main/docs/EIComponents.png)

## Integration Studio
- https://wso2.com/integration/integration-studio

##  Building a crud restful api with wso2-micro-integrator

**Create database.**
```
create database city_db character set latin1;
```
Run the following script.

```
use city_db
CREATE TABLE `city` (
`id` int(10) unsigned NOT NULL AUTO_INCREMENT,
`city` varchar(50) DEFAULT NULL,
PRIMARY KEY (`id`)
);
```

Now that we have created the database, let’s create the DataService using Integration Studio.

## Stop / remove all Docker containers

One liner to stop / remove all of Docker containers:
```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

