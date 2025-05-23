# 3 - Managing Data & Working with Volumes

Containers have 3 types of storage:

- Application Data: Code and Environment, written and provided by the developer. READ ONLY.
- Temporary Data/Container Data: Data that is created and used by the container. Stored in memory or temporary files, dynamic data.
- Persistent Data: Also produced by the running container, things like database or log files. But should not be lost if the container stops or is removed.

> Docker containers are a writeable layer on top of the image. The writable layer is where all changes to the container's filesystem are made. This includes any files created, modified, or deleted during the container's runtime. When a container is stopped or removed, the writable layer is also removed, along with any changes made to it.

## Anonymous and Named Volumes

As a first attempt, in the Dockerfile, I created a volume using the command `VOLUME ["/app/feedback"]`. This created an anonymous volume. However, it was not persistent, meaning that if I stopped and removed the container, the data in the volume would be lost. Anonymous volumes are managed by Docker and are not given a specific name.

To create a named volume, I can use the command `docker volume create {VOLUME_NAME}`. Or pass the `-v` flag to the `docker run` command. For example: `docker run -d --rm --name node-server-app -v feedback-v:/app/feedback -p 8080:80 node-server-data-volumes-app:volumes`

> The key difference between anonymous and named volumes is that named volumes are given a specific name and is not deleted when the container is removed.

## Volumes

Volumes are directories or files that are stored on the host filesystem outside of the container's filesystem. It's like a symlink between the host and the container. With that, I can share data between containers or persist data even if the container is removed.

Some volumes commands:

- `docker volume ls`: List all volumes.
- `docker volume create {VOLUME_NAME}`: Create a new volume.