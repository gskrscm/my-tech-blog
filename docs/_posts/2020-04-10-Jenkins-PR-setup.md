---
layout: post
title:  "How to trigger Jenkins job when Pull request (PR) raised ?"
date:   2020-04-10 10:10:58 +0530
author: siva
categories: Jenkins
image: assets/images/jenkins.png
---

In this we blog we will setup a Jenkins job which will trigger when a pull request on a github repository.

## Steps

* Pre requist, install github pull requester plugin in jenkins.
* Configure github pull requester pluin in jenkins managment.
* Create Jenkins job
* Configure Jenkins job
* Configure webhook in your github repository
* Validate payload on github.
* Raise PR to test the webhook.
