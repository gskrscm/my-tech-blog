---
layout: post
title:  "Docker Cheat Sheet, Tips and Tricks"
date:   2021-04-07 10:10:58 +0530
categories: Docker
featured: true
author: siva
image: assets/images/docker-jenkins.png
---

In this blog, we will cover Docker Cheat Sheet, Tips and Tricks.

## Getting Started commands

- To check version of docker

`docker --version`

- To check running containers

`docker ps`

- To check stopped containers

`docker ps -a`

- To check images present locally

`docker images`

- To remove all stopped containers 


## Docker container restart policy 

In order to enable a restart policy, you need to use the `--restart` argument when executing `docker run`

If you had an already running container that you wanted to change the restart policy for, you could use the docker update command to change that:

`docker update --restart unless-stopped container_id`

Then if you run a docker inspect for your container and look for RestartPolicy you should be able to see something like this:

```
    "RestartPolicy": {
        "Name": "unless-stopped",
```

There are a few other flags that you could specify to the --restart argument.

| Argument|	Description |
|---------| ------------ |
| no	| This is the default value, it means that the containers would not be restarted |
| on-failure | This would restart the container in case that there is an error and the container crashes |
| always |	Always restart the container if it stops |
| unless-stopped	| The container would always be restarted unless it was manually stopped |

For more information you could take a look at the official documentation here:
https://docs.docker.com/config/containers/start-containers-automatically/