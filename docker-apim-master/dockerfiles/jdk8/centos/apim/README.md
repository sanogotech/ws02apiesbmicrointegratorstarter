# Dockerfile for WSO2 API Manager #

This section defines the step-by-step instructions to build an [CentOS](https://hub.docker.com/_/centos/) Linux based Docker image for WSO2 API Manager 4.1.0.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v17.09.0 or above
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) client


## How to build an image and run

#### 1. Checkout this repository into your local machine using the following Git client command.

```
git clone https://github.com/wso2/docker-apim.git
```

> The local copy of the `dockerfiles/centos/apim` directory will be referred to as `AM_DOCKERFILE_HOME` from this point onwards.

#### 2. Build the Docker image.

- Navigate to `<AM_DOCKERFILE_HOME>` directory. <br>
  Execute `docker build` command as shown below.
```
docker build -t wso2am:4.1.0-centos-jdk8 .
```

> By default, the Docker image will prepackage the General Availability (GA) release version of the relevant WSO2 product.

#### 3. Running the Docker image.

```
docker run -it -p 9443:9443 -p 8243:8243 wso2am:4.1.0-centos-jdk8
```

> Here, only port 9443 (HTTPS servlet transport) and port 8243 (Passthrough or NIO HTTPS transport) have been mapped to Docker host ports.
You may map other container service ports, which have been exposed to Docker host ports, as desired.

#### 4. Accessing management console.

- To access the management console, use the docker host IP and port 9443.
    + `https://<DOCKER_HOST>:9443/carbon`
    
> In here, <DOCKER_HOST> refers to hostname or IP of the host machine on top of which containers are spawned.

## How to update configurations

Configurations would lie on the Docker host machine and they can be volume mounted to the container. <br>
As an example, steps required to change the port offset using `deployment.toml` is as follows:

#### 1. Stop the API Manager container if it's already running.

In WSO2 API Manager version 4.1.0 product distribution, `deployment.toml` configuration file <br>
can be found at `<DISTRIBUTION_HOME>/repository/conf`. Copy the file to some suitable location of the host machine, <br>
referred to as `<SOURCE_CONFIGS>/deployment.toml` and change the offset value (`[server]->offset`) to 1.

#### 2. Grant read permission to `other` users for `<SOURCE_CONFIGS>/deployment.toml`.

```
chmod o+r <SOURCE_CONFIGS>/deployment.toml
```

#### 3. Run the image by mounting the file to container as follows:

```
docker run \
-p 9444:9444 \
-p 8244:8244 \
--volume <SOURCE_CONFIGS>/deployment.toml:<TARGET_CONFIGS>/deployment.toml \
wso2am:4.1.0-centos-jdk8
```

> In here, <TARGET_CONFIGS> refers to /home/wso2carbon/wso2am-4.1.0/repository/conf folder of the container.

## How to build a Docker image with multi architecture support

The above wso2am:4.1.0-centos-jdk8 image will only be supported for the CPU architecture of your current machine. Docker buildx plugin can be used to build wso2am:4.1.0-centos-jdk8 image to support any CPU architecture.

#### 1. Install [Docker Buildx](https://docs.docker.com/buildx/working-with-buildx/)

#### 2. Install [QEMU Emulators](https://github.com/tonistiigi/binfmt)
```
docker run -it --rm --privileged tonistiigi/binfmt --install all
```

#### 3. Create, switch and inspect a new builder
```
docker buildx create --name wso2ambuilder
```
```
docker buildx use wso2ambuilder
```
```
docker buildx inspect --bootstrap
```
#### 4. Build and push 

```
docker buildx build --platform linux/amd64,linux/arm64 -t <DOCKER_USERNAME>/wso2am:4.1.0-centos-jdk8-multiarch --push .
```

> - Here <DOCKER_USERNAME> is a valid Docker or Dockerhub username.
> - Use command "docker login" to authenticate first if it fails to push.
> - You can specify any number of platforms to support --platform flag
> - Use command "docker buildx ls" to see list of existing builders and supported platforms.
> - Please note we have only tested this for linux/amd64 and linux/arm64 platforms only

#### 5. Run
```
docker run -it -p 9443:9443 -p 8243:8243 <DOCKER_USERNAME>/wso2am:4.1.0-centos-jdk8-multiarch
```
> Docker will pull the suitable image for the architecture and run

## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
* [Docker multi architecture build reference](https://docs.docker.com/desktop/multi-arch/)
* [Docker buildx reference](https://docs.docker.com/buildx/working-with-buildx/)
