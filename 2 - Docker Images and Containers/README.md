# 1 - Docker Images and Containers

To create a container from an image I can use the command: `docker run {IMAGE_NAME}`. If the image is not available locally, Docker will try to pull it from the Docker Hub.

To attach a terminal to the container, I can use the `-it` flag. This allows me to interact with the container's shell
The -it flag is a combination of two flags:

- `-i` (interactive): Keeps STDIN open even if not attached.
- `-t` (tty): Allocates a pseudo-TTY, which gives me a terminal interface.

## Building a Docker Image

To build a Docker image, I can use the command: `docker build -t {IMAGE_NAME:TAG} .` where the `.` indicates the current directory.

For more information on the Dockerfile, refer to `nodejs-app-starting-setup` application.

> Images are read-only templates used to create create containers. Any changes made to the source code require a new image to be built.

I can inspect the image using the command `docker image inspect {IMAGE_NAME}`. This command provides detailed information about the image, including its layers, environment variables, exposed ports, and other configuration. This is useful when I want to understand better images from Docker Hub or when I need to troubleshoot issues with my own images.

## Running the Created Image

To run the created image, I used the command `docker run node-app-test:latest -p 80:80 --name node-app` where `node-app-test` is the name of image.

> If I wanted to interact with the container, let's say by typing, I can add the `-it` flag to the command: `docker run -it ...`

> If I wanted the container to be removed after it stops, I can add the `--rm` flag to the command: `docker run --rm ...`

Even though in the Dockerfile I exposed the port 80, in order to access the application from the host machine, I need to map the container's port to a port on the host machine. This is done using the `-p` flag, which maps the container's port to a port on the host machine.

When a container is already created and I want to run it again, I can use the command `docker start {CONTAINER_ID}`.

To attach a terminal to a running container to execute commands, I can use:

- `docker exec -it {CONTAINER_ID} /bin/bash`: This command opens a bash shell in the running container.

> `docker exec` is used to run a command in a running container. The `-it` flag allows me to interact with the container's shell.

To attach to a running container, as if I had executed `docker run ...`, I can use the command `docker attach {CONTAINER_ID}`. This is useful for debugging or monitoring the container's output.

Additionally, I can use the command `docker logs {CONTAINER_ID}` to view the logs of a running container. Along with the `-f` flag, I can follow the logs in real-time.

I can copy files from the host machine to the container using the command `docker cp {SOURCE} {CONTAINER_ID}:{DESTINATION}`. For example, configuration files that I want to add to the container after it has been created.

## Image Layers

Docker images are made up of layers. Each layer represents a set of file changes.

I can create layers by modifying running containers, like installing a new package or adding a file, and then commit those changes with `docker container commit base-container new-container`.

Or whenever I execute a command in the Dockerfile, a new layer is created. For example, when I run `COPY . ./`, a new layer is created. Layers are cached, so if I make a change to a layer, only that layer and the layers above it need to be rebuilt.

Whenever one of the layers is changed, all subsequent layers are invalidated and need to be rebuilt. This is why it's important to order the commands in the Dockerfile in a way that minimizes the number of layers that need to be rebuilt.

## Observations
