---
layout: post
title:  "[Docker] Rebuilding the Docker Image on change"
date:   2024-02-28
categories: ["docker", "docker-compose"]
draft: false
---

I usually do my local development with docker containers and especially `docker-compose`. In the olden days, I would have to sync the changes manually, but now Docker has released [Docker Compose Watch](https://docs.docker.com/compose/file-watch/) which lets you rebuild the application as soon as there are changes registered and rebuilds the docker container.

Let's take for example a python application with `app.py` as the sole application running there. The Composefile would be as follow

```yaml
services:
  test-app:
    build:
      context: .
    ports:
      - 8000:8000
    develop:
      watch:
        - action: sync
          path: .
		  target: /app
        - action: rebuild
          path: ./app.py
```

After that, simply run `docker compose watch`. The Docker daemon will be watching your codes for a change and rebuild upon change. 