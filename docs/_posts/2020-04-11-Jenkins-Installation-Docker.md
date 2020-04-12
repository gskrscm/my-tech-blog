---
layout: post
title:  "How to install and run Jenkins using docker and docker compose?"
date:   2020-04-11 10:10:58 +0530
categories: Jenkins
featured: true
author: siva
image: assets/images/docker-jenkins.png
---

In this blog, we will install jenkins using docker. Also, we will persist data out side of the container. 
We will also learn how to install using docker compose. With the help of docker compose, it will be very easy to run Jenkins. 

## Pre-requists
* Docker
* Docker-Compose

## Steps

1) First pull the jenkins docker image from the docker hub repository.

`docker pull jenkins/jenkins`
```shell 
docker pull jenkins/jenkins
Using default tag: latest
latest: Pulling from jenkins/jenkins
3192219afd04: Pull complete
17c160265e75: Pull complete
cc4fe40d0e61: Pull complete
9d647f502a07: Pull complete
d108b8c498aa: Pull complete
1bfe918b8aa5: Pull complete
dafa1a7c0751: Pull complete
db010e3cb6ec: Pull complete
6c9119db5ee3: Pull complete
6bbf3e9165cb: Pull complete
869e8e75811f: Pull complete
a87c1fec27a9: Pull complete
60086e34e824: Pull complete
64ae86b76ca5: Pull complete
3ffb228af6fb: Pull complete
8b4110b33e02: Pull complete
da04ea39a0b4: Pull complete
760fb652ccb0: Pull complete
95ea551cf255: Pull complete
Digest: sha256:8e7b01c96c848058a2820834155c0a4756bc16f83ce417da6bfcfa469ae73fae
Status: Downloaded newer image for jenkins/jenkins:latest
docker.io/jenkins/jenkins:latest
```

* Check the downloaded image  using `docker images` command
```shell
docker images
REPOSITORY                                                   TAG                 IMAGE ID            CREATED             SIZE
jenkins/jenkins                                              latest              3b38d20c7a23        5 days ago          619MB
```

2) Next, we have to start the container from jenkins image using below command.

`docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts`

```shell
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
Running from: /usr/share/jenkins/jenkins.war
webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-04-12 06:24:14.008+0000 [id=1]	INFO	org.eclipse.jetty.util.log.Log#initialized: Logging initialized @1118ms to org.eclipse.jetty.util.log.JavaUtilLog
2020-04-12 06:24:14.292+0000 [id=1]	INFO	winstone.Logger#logInternal: Beginning extraction from war file
2020-04-12 06:24:16.324+0000 [id=1]	WARNING	o.e.j.s.handler.ContextHandler#setContextPath: Empty contextPath
2020-04-12 06:24:16.460+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: jetty-9.4.z-SNAPSHOT; built: 2019-05-02T00:04:53.875Z; git: e1bc35120a6617ee3df052294e433f3a25ce7097; jvm 1.8.0_232-b09
2020-04-12 06:24:16.985+0000 [id=1]	INFO	o.e.j.w.StandardDescriptorProcessor#visitServlet: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
2020-04-12 06:24:17.114+0000 [id=1]	INFO	o.e.j.s.s.DefaultSessionIdManager#doStart: DefaultSessionIdManager workerName=node0
2020-04-12 06:24:17.114+0000 [id=1]	INFO	o.e.j.s.s.DefaultSessionIdManager#doStart: No SessionScavenger set, using defaults
2020-04-12 06:24:17.126+0000 [id=1]	INFO	o.e.j.server.session.HouseKeeper#startScavenging: node0 Scavenging every 600000ms
2020-04-12 06:24:17.822+0000 [id=1]	INFO	hudson.WebAppMain#contextInitialized: Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
2020-04-12 06:24:18.133+0000 [id=1]	INFO	o.e.j.s.handler.ContextHandler#doStart: Started w.@26be6ca7{Jenkins v2.204.1,/,file:///var/jenkins_home/war/,AVAILABLE}{/var/jenkins_home/war}
2020-04-12 06:24:18.195+0000 [id=1]	INFO	o.e.j.server.AbstractConnector#doStart: Started ServerConnector@19932c16{HTTP/1.1,[http/1.1]}{0.0.0.0:8080}
2020-04-12 06:24:18.196+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: Started @5308ms
2020-04-12 06:24:18.199+0000 [id=21]	INFO	winstone.Logger#logInternal: Winstone Servlet Engine v4.0 running: controlPort=disabled
2020-04-12 06:24:20.211+0000 [id=28]	INFO	jenkins.InitReactorRunner$1#onAttained: Started initialization
2020-04-12 06:24:20.266+0000 [id=31]	INFO	jenkins.InitReactorRunner$1#onAttained: Listed all plugins
2020-04-12 06:24:22.235+0000 [id=32]	INFO	jenkins.InitReactorRunner$1#onAttained: Prepared all plugins
2020-04-12 06:24:22.258+0000 [id=32]	INFO	jenkins.InitReactorRunner$1#onAttained: Started all plugins
2020-04-12 06:24:22.273+0000 [id=28]	INFO	jenkins.InitReactorRunner$1#onAttained: Augmented all extensions
2020-04-12 06:24:23.180+0000 [id=29]	INFO	jenkins.InitReactorRunner$1#onAttained: Loaded all jobs
2020-04-12 06:24:23.224+0000 [id=46]	INFO	hudson.model.AsyncPeriodicWork#lambda$doRun$0: Started Download metadata
2020-04-12 06:24:23.247+0000 [id=46]	INFO	hudson.util.Retrier#start: Attempt #1 to do the action check updates server
2020-04-12 06:24:24.751+0000 [id=28]	INFO	o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@44bbbe2d: display name [Root WebApplicationContext]; startup date [Sun Apr 12 06:24:24 UTC 2020]; root of context hierarchy
2020-04-12 06:24:24.752+0000 [id=28]	INFO	o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@44bbbe2d]: org.springframework.beans.factory.support.DefaultListableBeanFactory@3dbc2e3a
2020-04-12 06:24:24.778+0000 [id=28]	INFO	o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@3dbc2e3a: defining beans [authenticationManager]; root of factory hierarchy
2020-04-12 06:24:25.039+0000 [id=28]	INFO	o.s.c.s.AbstractApplicationContext#prepareRefresh: Refreshing org.springframework.web.context.support.StaticWebApplicationContext@71a0bf49: display name [Root WebApplicationContext]; startup date [Sun Apr 12 06:24:25 UTC 2020]; root of context hierarchy
2020-04-12 06:24:25.040+0000 [id=28]	INFO	o.s.c.s.AbstractApplicationContext#obtainFreshBeanFactory: Bean factory for application context [org.springframework.web.context.support.StaticWebApplicationContext@71a0bf49]: org.springframework.beans.factory.support.DefaultListableBeanFactory@3a9f051d
2020-04-12 06:24:25.042+0000 [id=28]	INFO	o.s.b.f.s.DefaultListableBeanFactory#preInstantiateSingletons: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@3a9f051d: defining beans [filter,legacy]; root of factory hierarchy
2020-04-12 06:24:25.433+0000 [id=28]	INFO	jenkins.install.SetupWizard#init:

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

37cd149a0fcf4505be3f9858f72faaad

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************

2020-04-12 06:24:33.062+0000 [id=28]	INFO	hudson.model.UpdateSite#updateData: Obtained the latest update center data file for UpdateSource default
2020-04-12 06:24:33.401+0000 [id=32]	INFO	jenkins.InitReactorRunner$1#onAttained: Completed initialization
2020-04-12 06:24:33.433+0000 [id=20]	INFO	hudson.WebAppMain$3#run: Jenkins is fully up and running
```

The jenkins process starts and the process runs in the foreground. To make it run the jenkins process in the background, use `-d` option for the same command. 

`docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts`

```shell
docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
5babc6c7f8d971a623be95f75a7e50f392cebae2a071c38e07b249a0fd226a33
```
We can browse the jenkins at `http://localhost:8080`
![jenkins-login-page]({{ site.baseurl }}/assets/images/jenkins-login.png)

To unlock the jenkins, jenkins displays admin password in the jenkins log or we can copy the `/var/jenkins_home/secrets/initialAdminPassword` file in host machine and read it. To copy the file `docker cp <container id>:/var/jenkins_home/secrets/initialAdminPassword pass.txt`

```shell
docker cp 5babc6c7f8d9:/var/jenkins_home/secrets/initialAdminPassword pass.txt
cat pass.txt
8ebf0301d68740608cab990b8a1b524b
```

By using `docker ps` command we can see running docker containers.
```shell
docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                              NAMES
5babc6c7f8d9        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   10 seconds ago      Up 9 seconds        0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   stupefied_bartik
```

To see jenkins container logs run `docker logs <jenkins container id/name>`, to see logs continuously add `-f` option at the end of the command i.e, `docker logs <jenkins container id/name> -f`.

To stop the jenkins container run `docker stop <jenkins container id/name>`

3) Persist jenkins home data directory. When the jenkins container deleted, the jobs, plugins and global configs also be lost. So, we have to persist the jenkins home directory to host machine. Using option `-v <host machine full path>:<jenkins home directory on container>`

```shell
 docker run -d -v /Users/siva/Play/docker/jenkins/data:/var/jenkins_home \
                        -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```

4) Running jenkins via docker compose. 

* Create `docker-compose.yml` file and copy below content

```yaml
version: '3'
services:
  jenkins:
    image: 'jenkins/jenkins:lts'
    ports:
      - '8080:8080'
      - '50000:50000'
    volumes:
      - 'jenkins_home:/var/lib/jenkins_home'
volumes:
  jenkins_home:
    driver: local
```
* Run `docker-compose up -d` command to bring up jenkins and send process background.
* Run `docker-compose down` command to remove the container
* Run `docker-compose stop` command to stop the container


If you face any issues, setting jenkins using docker, please share error in the comments. We will try to help you.