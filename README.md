﻿# MicroART
**MicroART-Tool** is the first prototype implementation of a more generic approach named **MicroART**.

The overall approach is presented in the [Towards Recovering the Software Architecture of Microservice-based Systems](http://www.ivanomalavolta.com/files/papers/AMS_2017.pdf) published at [1st International Workshop on Architecting with MicroServices, 2017](https://ams2017.github.io/). The [slides](https://www.slideshare.net/paolodifrancesco/towards-recovering-the-software-architecture-of-microservicebased-systems) of the approach are also available.

The details on the tools are presented in [MicroART: A Software Architecture Recovery Tool for Maintaining Microservice-based Systems](http://www.ivanomalavolta.com/files/papers/ICSA_2017_tool.pdf) published at [1st International Conference on Software Architecture (ICSA), Tool Papers,2017]. The [slides](https://www.slideshare.net/paolodifrancesco/microart-a-software-architecture-recovery-tool-for-maintaining-microservicebased-systems) of the tool are also available.


## Disclaimer 
**This software is published for academic and non-commercial use only.**

## Setup
The prototype has been tested in the following conditions.

### Environment

    Operative System Host Machine
    Linux Kubuntu 16.04
    Linux kernel version 4.4.0-31-generic
    Docker Client/ Server

### Client

    Version:      1.13.0
    API version:  1.25
    Go version:   go1.7.3
    Git commit:   49bf474
    Built:        Tue Jan 17 09:58:26 2017
    OS/Arch:      linux/amd64

### Server

    Version:      1.13.0
    API version:  1.25 (minimum version 1.12)
    Go version:   go1.7.3
    Git commit:   49bf474
    Built:        Tue Jan 17 09:58:26 2017
    OS/Arch:      linux/amd64
    Experimental: false

### Docker Machine
    docker-machine version 0.9.0, build 15fd4c7

### Docker Compose
    docker-compose version 1.10.0, build 4bd6f1

### Virtual Box version 5.0.32

### Development environment
```
Eclipse Neon .2 release 4.6.2
Apache Maven 3.3.9
Nodejs  4.2.6
NPM 3.5.2
Openjdk 1.8.0_121
```

# Usage

## Docker Installation

```
    sudo apt-get update
    sudo apt-get install curl linux-image-extra-$(uname -r) linux-image-extra-virtual
    sudo apt-get install apt-transport-https ca-certificates
    curl -fsSL https://yum.dockerproject.org/gpg | sudo apt-key add -
    apt-key fingerprint 58118E89F3A912897C070ADBF76221572C52609D
    sudo apt-get install software-properties-common
    sudo add-apt-repository \
      "deb https://apt.dockerproject.org/repo/ \
      ubuntu-$(lsb_release -cs) \
      main"
    sudo apt-get update
    sudo apt-get -y install docker-engine
    apt-cache madison docker-engine
```

In this project we have used Docker version 1.13.0-0
```
    sudo apt-get -y install docker-engine=<1.13.0-0>
```

Manage Docker as a non-root user
```
    sudo groupadd docker
    sudo usermod -aG docker $USER
```
A session logout may be needed.


Configure Docker to start on boot
```
    sudo systemctl enable docker
```

Docker-Machine
```
    curl -L https://github.com/docker/machine/releases/download/v0.9.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
    chmod +x /tmp/docker-machine &&
    sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
    docker-machine version
``` 



Docker-compose
```
    curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
```

To check the docker version you can run
```
    docker-compose --version
```

## AcmeAIR Benchmark

We have used [AcmeAir](https://github.com/acmeair/acmeair-nodejs) as public benchmark for our prototype.
AcmeAir uses Docker and is a simple microservice-based application.



### Setup AcmeAir
```
    git clone https://github.com/wasperf/acmeair-nodejs.git
    cd acmeair-nodejs
    npm install
    cd node_modules/.bin
    npm install
    cd ..
    
    docker network create --driver bridge my-net
    set NETWORK=my-net && export NETWORK=my-net 
```


### Customization

Before being able to build the DockerFiles, it is necessary to modify each Dockerfile in the root by *replacing the first row* with the following. 
```
    FROM ibmcom/ibmnode
``` 

### Build 
The Dockerfile build may take a while
``` 
    docker-compose build
    docker-compose up
``` 

The *up* command starts the services in only one window, and in the terminal you can look at the application logs and communication logs.

Can take up to one or two minute for all  services to be ready to run. You can visit the localhost page of the system at:
``` 
     http:://127.0.0.1:80/main/acmeair
``` 

If the page is not ready, the system might require more time or try the following Docker-machine commands:
```
    docker-machine create --driver virtualbox default
    eval "$(docker-machine env default)"
```

Now AcmeAir is ready to be used, but note that the *Support* service is not working.



## Running MicroART 


### Run MicroART Recovery process

[Download and run Eclipse](http://www.eclipse.org/neon/).
Clone this repository and copy it inside the given Eclipse workspace.

```
    git clone https://github.com/microart/microART-Tool.git
    cd microART

    Open eclipse Neon.    
    Set a Workspace.    
    Go to *Project Explorer*, Right-Click and choose *Import*.Select *Existing Maven Project*

    Select the proper directory and wait until the maven build is completed.
    
    Right-Click on project -> *Properties*
    
    Click on *Java Build Path*
    
    Click on *Add JAR*
    
    Add the JAR inside the project, in src/main
    
  ```
    To run just click Start on the top bar of Eclipse.
    
### Run MicroART Presentation process   

[Download and run Eclipse Epsilon](http://www.eclipse.org/epsilon/).

```    
    Create a workspace 
    Import, from *General source* all the project present in the directory PresentationProject
    Right Click on project called MinimalDSL, Run as an Eclipse Application
    Wait until a new runtime eclipse istance is ready.
    
    Once the new eclipse istance is started, create a runtime directory in the workspace and import from *General source*, the project into RunTime/runtime-EclipseApplication

```