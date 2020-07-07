---
title: "Access Host From Docker Container in Macos"
date: 2020-07-07T13:09:08+05:30
slug: ""
description: ""
keywords: [docker, gateway, docker-for-mac]
draft: false
tags: [docker, gateway, docker-for-mac]
math: false
toc: false
---

If you are running a docker container and if you need to access a service running on the host for testing, the option in Ubuntu and other linux distros is to use the docker0 interface. This will not work in MacOS as Docker-Desktop-for-Mac networking is programmed different. 

To make this scenario work, Docker Desktop for Mac provides a work around and it is pretty simple. The gateway is provided with an internal DNS name `host.docker.internal`. This resolves to the host's volatile internal IP. Docker also provides an alternate `gateway.docker.internal` DNS name to access the host from containers.

More info is available at [Docker For Mac documentation](https://docs.docker.com/docker-for-mac/networking/)