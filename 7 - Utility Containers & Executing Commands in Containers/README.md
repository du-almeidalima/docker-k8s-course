# 7 - Utility Containers & Executing Commands in Containers

This section covers the use of utility containers to run projects or execute commands without needing to install dependencies on the host system.

## Executing Commands in Containers

To execute commands in a utility container, I can do the following:

- Use the `docker run [IMAGE] [THE_COMMAND]` command with the command I want to run. This will override the default CMD in the Dockerfile.
- Use the `docker exec -it [CONTAINER] [THE_COMMAND]` command to run a command in an already running container.
- Use the `docker attach [CONTAINER]` command to attach to a running container, allowing me to interact with it as if I were in a terminal session.
- Use the `docker-compose run [SERVICE] [THE_COMMAND]` command to run a command in a service defined in a `docker-compose.yml` file.
  - `docker-compose run` creates a new container, run the command, and then stops the container. To also remove the container after it stops, I can use the `--rm` flag.

## Dockerfile CMD and ENTRYPOINT

The `CMD` and `ENTRYPOINT` instructions in a Dockerfile define the default command that runs when a container starts.

- `CMD` specifies the default command to run when the container starts, but it can be overridden by providing a command in the `docker run` command.
- `ENTRYPOINT` sets the command that will always run when the container starts, and it cannot be overridden by providing a command in the `docker run` command.
- If both `CMD` and `ENTRYPOINT` are specified, the `CMD` will be passed as arguments to the `ENTRYPOINT`.