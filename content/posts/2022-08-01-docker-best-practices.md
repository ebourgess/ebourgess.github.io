---
layout: post
title:  "[Docker] Best practices"
date:   2022-08-01
categories: ["docker"]
draft: false
---

8 of the best practices that we should be following are:

1.  Using official images from docker
2.  Using a fixed version and not the latest from the images
3.  Don't use a full-blown OS in Docker images (Try to use Alpine to save some space)
4.  Optimize caching image layers (Restructure Dockerfile to have the commands from least changing to most frequently changing commands)
5.  Use .dockerignore file to save space and lower the image's size
6.  Make use of [multistage builds](https://docs.docker.com/develop/develop-images/multistage-build/) (Separate build from runtime stage)
7.  Don't use root as a user in the container (Create a dedicated user and group in the Dockerfile)
8.  Scan Images for vulnerabilities (Integrate docker container scanning in CI/CD)