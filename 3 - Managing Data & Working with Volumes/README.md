# 3 - Managing Data & Working with Volumes

Containers have 3 types of storage:

- Application Data: Code and Environment, written and provided by the developer. READ ONLY.
- Temporary Data/Container Data: Data that is created and used by the container. Stored in memory or temporary files, dynamic data.
- Persistent Data: Also produced by the running container, things like database or log files. But should not be lost if the container stops or is removed.

> Docker containers are a writeable layer on top of the image. The writable layer is where all changes to the container's filesystem are made. This includes any files created, modified, or deleted during the container's runtime. When a container is stopped or removed, the writable layer is also removed, along with any changes made to it.

## Volumes

Volumes are directories or files that are stored on the host filesystem outside of the container's filesystem. It's like a symlink between the host and the container. With that, I can share data between containers or persist data even if the container is removed.

Some volumes commands:

- `docker volume ls`: List all volumes.
- `docker volume create {VOLUME_NAME}`: Create a new volume.

Volumes are managed by Docker and are stored in a directory which is unknown to the user. By default, volumes are stored in `/var/lib/docker/volumes/` on Linux systems. This directory is not meant to be accessed directly by users.

## Anonymous and Named Volumes

As a first attempt, in the Dockerfile, I created a volume using the command `VOLUME ["/app/feedback"]`. This created an anonymous volume. However, it was not persistent, meaning that if I stopped and removed the container, the data in the volume would be lost. Anonymous volumes are managed by Docker and are not given a specific name.

To create a named volume, I can use the command `docker volume create {VOLUME_NAME}`. Or pass the `-v` flag to the `docker run` command. For example:

```shell
docker run -d --rm --name node-server-app -v feedback-v:/app/feedback -p 8080:80 node-server-data-volumes-app:volumes
```

> The key difference between anonymous and named volumes is that named volumes are given a specific name and is not deleted when the container is removed.

## BIND MOUNTS

The difference between volumes and bind mounts is that bind mounts allow you to specify a specific directory on the host filesystem to be mounted into the container.

It's good for persistent but editable data, like the source code of an application.

To create a bind mount, I can use pretty much the same command as for volumes, but with the full path to the directory on the host filesystem. For example:

```shell
docker run -d --rm --name node-server-app -p 8080:80 -v feedback-v:/app/feedback -v "/Users/kisdev/Development/courses/docker-k8s-course/3 - Managing Data & Working with Volumes/data-volumes-01-starting-setup:/app" -v /app/node_modules node-server-data-volumes-app:volumes
```

> **IMPORTANT:** When using bind mounts, it will replace the contents of the directory in the container with the contents of the directory on the host filesystem. This means if previous steps in the Dockerfile created files in that directory, they will be removed and replaced with the contents of the directory on the host filesystem. To avoid this, I can use anonymous volumes to map specific directories that I want to persist. Docker has when merging volumes that the more specific volume will take precedence over the less specific one. So if I have a bind mount for `/app` and an anonymous volume for `/app/node_modules`, the anonymous volume will take precedence and the contents of `/app/node_modules` will not be replaced by the contents of the bind mount.
