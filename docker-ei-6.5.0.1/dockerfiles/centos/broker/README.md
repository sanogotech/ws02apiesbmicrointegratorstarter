# Dockerfile for Broker profile of WSO2 Enterprise Integrator #
This section defines the step-by-step instructions to build an [CentOS](https://hub.docker.com/_/centos/) Linux based Docker image
Broker profile for WSO2 Enterprise Integrator 6.5.0.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v17.09.0 or above


## How to build an image and run
##### 1. Checkout this repository into your local machine using the following Git command.
```
git clone https://github.com/wso2/docker-ei.git
```

>The local copy of the `dockerfiles/centos/broker` directory will be referred to as `BROKER_DOCKERFILE_HOME` from this point onwards.

##### 2. Add Broker profile distribution and MySQL connector to `<BROKER_DOCKERFILE_HOME>/files`.

- Download [AdoptOpenJDK 8](https://adoptopenjdk.net/) and extract it to `<BROKER_DOCKERFILE_HOME>/files`.
- Download [WSO2 Enterprise Integrator 6.5.0 distribution](https://wso2.com/integration/) distribution.
Extract the product distribution and execute the `<EI_HOME>/bin/profile-creator.sh` to generate the Broker
profile distribution.

```
./<EI_HOME>/bin/profile-creator.sh
``` 

Extract the generated profile distribution to `<BROKER_DOCKERFILE_HOME>/files`.
- Download [MySQL Connector/J](https://downloads.mysql.com/archives/c-j)
and copy that to `<BROKER_DOCKERFILE_HOME>/files`.
- Once all of these are in place, it should look as follows:

  ```bash
  <BROKER_DOCKERFILE_HOME>/files/wso2ei-6.5.0/
  <BROKER_DOCKERFILE_HOME>/files/mysql-connector-java-<version>-bin.jar
  ```
  
>Please refer to [WSO2 Update Manager documentation]( https://docs.wso2.com/display/WUM300/WSO2+Update+Manager)
in order to obtain latest bug fixes and updates for the product.

##### 3. Build the Docker image.
- Navigate to `<BROKER_DOCKERFILE_HOME>` directory. <br>
  Execute `docker build` command as shown below.
    + `docker build -t wso2ei-broker:6.5.0-centos .`
    
##### 4. Running the Docker image.
- `docker run -p 9446:9446 ...all-port-mappings-here... wso2ei-broker:6.5.0`
>Here, only port 9446 (HTTPS servlet transport) has been mapped to a Docker host port.
You may map other container service ports, which have been exposed to Docker host ports, as desired.

##### 5. Accessing management console.
- To access the management console, use the docker host IP and port 9446.
    + `https:<DOCKER_HOST>:9446/carbon`
    
>In here, <DOCKER_HOST> refers to hostname or IP of the host machine on top of which containers are spawned.


## How to update configurations
Configurations would lie on the Docker host machine and they can be volume mounted to the container. <br>
As an example, steps required to change the port offset using `carbon.xml` is as follows.

##### 1. Stop the Broker profile container if it's already running.
In Broker profile product distribution, `carbon.xml` configuration file can be found at `<DISTRIBUTION_HOME>/wso2/broker/conf`.
Copy the file to some suitable location of the host machine, referred to as `<SOURCE_CONFIGS>/carbon.xml` and increase
the offset value under ports by 1.

##### 2. Grant read permission to `other` users for `<SOURCE_CONFIGS>/carbon.xml`
```
chmod o+r <SOURCE_CONFIGS>/carbon.xml
```

##### 3. Run the image by mounting the file to container as follows.
```
docker run \
-p 9447:9447 \
--volume <SOURCE_CONFIGS>/carbon.xml:<TARGET_CONFIGS>/carbon.xml \
wso2ei-broker:6.5.0-centos
```

>In here, <TARGET_CONFIGS> refers to /home/wso2carbon/wso2ei-6.5.0/wso2/broker/conf folder of the container.


## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
