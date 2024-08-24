---
layout: post
title:  "[AI] Installing Ollama on Fedora"
date:   2024-08-24
summary: "Steps I followed to install Ollama on my Fedora machine"
slug: "installing-ollama-on-fedora"
authors: ["Elias Bourgess"]
categories: ["AI"]
draft: false
---

## Why am I doing this?

While going through my Github feed today, I saw that [Martin](https://newsletter.nord-nord-sec.de/) had created a [new repository](https://github.com/nord-nord-sec/extwis_summaries.git) containing AI generated summary from an article. When I looked more into it, I saw that the article was actually using [Fabric](https://github.com/danielmiessler/fabric) to use AI in the CLI.

As I was looking into the Github Repository linked there, I saw a [video](https://www.youtube.com/watch?v=UbDyjIIGaxQ) by NetworkChuck that explained how to use this. And I guess seeing things running in front of you would tempt you into trying it yourself. And with no cost actually to this, I think that it's a good thing to test out.

Now since I need to have an actual LLM to run `Fabric`, and I don't really want to be paying for another subscription that I might forget to use or not use as often, I will have to run [Ollama](https://ollama.com/) on my machine. But since I don't want to be running it directly on my laptop, I decided to just  run it in `Docker` by getting the image from [DockerHub](https://hub.docker.com/r/ollama/ollama)

## Running Ollama In Docker

### Installing Nvidia Container Toolkit

Since I am using an Nvidia GPU, I will have to install the `nvidia-container-toolkit`. In order to do so, these are the commands that I followed.

**Configuring the repository**

```bash
curl -s -L https://nvidia.github.io/libnvidia-container/stable/rpm/nvidia-container-toolkit.repo \
    | sudo tee /etc/yum.repos.d/nvidia-container-toolkit.repo
```

**Installing `nvidia-container-toolkit`**

```bash
sudo dnf update
sudo dnf install -y nvidia-container-toolkit
```

**Configuring Docker to use Nvidia Driver**

```shell
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

### Running the Docker container

In order to run `Ollama` on my machine, I went ahead and installed it via `Docker` by running the following command

```shell
docker run -d --device /dev/kfd --device /dev/dri -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:rocm
```

### Running Model locally

```shell
docker exec -it ollama ollama run llama3.1
```

## Running Ollama locally on server

### Installation

As for some reason Docker keeps timing out when I try to pull the `llama`  model, I decided to just install it locally. So to do that, I will be using  this script to run it.

```shell
curl -fsSL https://ollama.com/install.sh | sh
```

However, for some reason the `CUDA` wasn't installed, so I have to install it manually. In order to do that, I will have to install the module `nvidia-driver:latest-dkms` as follows:

```bash
sudo dnf module install nvidia-driver:latest-dkms
```

And then enable the service if not enabled and restart it

```shell
sudo systemctl enable ollama
sudo systemctl restart ollama
```

### Running the Model locally 

```shell
ollama run llama3.1
```
