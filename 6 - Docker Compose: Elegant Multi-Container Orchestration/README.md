# Docker Compose: Elegant Multi-Container Orchestration

Creating multiple Docker containers that work together can be complex, as seen in the previous section 5.
Docker Compose allows replacing Docker Build and Docker Run commands with a single file that orchestrates the containers.

This allows us to run multiple containers with a single command.

## Overview

To demonstrate Docker Compose, this section will reuse the projects from the previous section. But instead of running each container separately, their configs will be defined in a `docker-compose.yml` file.

## Commands

This command will start all the containers defined in the `docker-compose.yml` file in detached mode.

```shell
docker compose up -d
```

This command will stop and remove all the containers defined in the `docker-compose.yml` file.

```shell
docker compose down
```

This command will build the images defined in the `docker-compose.yml` file.

```shell
docker compose build
```
