---
layout: post
title:  "[ElasticSearch] How to fix issue with elastic search container on windows with WSL and Powershell"
date:   2023-05-02
authors: ["Elias Bourgess"]
slug: "fixing-issue-with-elastic-search-on-windows"
categories: ["DevOps"]
tags: ["elasticsearch", "docker", wsl"]
draft: false
---

### Memory issue

- Open `PowerShell` 
- Run `wsl -d docker-desktop` inside PowerShell
- Run `sysctl -w vm.max_map_count=262144` inside the terminal 
- Run `exit` to exit the VM's terminal

---

### Initial setup 

Due to permission issues on Windows and WSL, there are some extra steps that we need to have. First add the following to the `docker-compose.yml` 

```yaml
...
 - xpack.security.enabled=false
...
```

Then run the following command in the terminal

```shell
sudo chown -R 1000:1000 .data/es-data-folder
```
