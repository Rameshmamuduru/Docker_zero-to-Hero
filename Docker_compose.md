# What is Docker-Compose?

Docker Compose is a tool used to define and run multi-container Docker applications in a simple and structured way. Instead of running many docker run commands, you describe all your services, networks, and volumes in one file (docker-compose.yml) and start everything with one command.

# Key Components of Docker Compose
# 1. docker-compose.yml

A YAML file where you define:

Services (containers)

Ports

Volumes

Networks

Environment variables

Dependencies

# 2. Services

Each container = one service.

# 3. One-command lifecycle

Start everything → docker-compose up

Stop everything → docker-compose down

Restart → docker-compose restart

# Installation of docker:


``` bash
sudo apt update
sudo apt install docker.io -y
sudo chmod -aG docker $USER
newgrp docker
```

# Docker Compose installation:

``` bash
sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.29.2/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
docker compose version

```
