# 3 - Managing Data & Working with Volumes, Env Variables and Arguments

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

## Bind Mounts

The difference between volumes and bind mounts is that bind mounts allow you to specify a specific directory on the host filesystem to be mounted into the container.

It's good for persistent but editable data, like the source code of an application.

To create a bind mount, I can use pretty much the same command as for volumes, but with the full path to the directory on the host filesystem. For example:

```shell
docker run -d --rm --name node-server-app -p 8080:80 -v feedback-v:/app/feedback -v "/project_dir/data-volumes-01-starting-setup:/app" -v /app/node_modules -v /app/temp node-server-data-volumes-app:volumes
```

> **IMPORTANT:** When using bind mounts, it will replace the contents of the directory in the container with the contents of the directory on the host filesystem. This means if previous steps in the Dockerfile created files in that directory, they will be removed and replaced with the contents of the directory on the host filesystem. To avoid this, I can use anonymous volumes to map specific directories that I want to persist. Docker has a strategy when merging volumes where the more specific volume will take precedence over the less specific one. So if I have a bind mount for `/app` and an anonymous volume for `/app/node_modules`, the anonymous volume will take precedence and the contents of `/app/node_modules` will not be replaced by the contents of the bind mount.

**Read Only**: If I want to make the bind mount read-only, I can add the `:ro` flag at the end of the volume definition. This is useful when I want to share data with the container but not allow the container to modify it, only the host. For example:

```shell
docker run -d --rm --name node-server-app -p 8080:80 -v feedback-v:/app/feedback -v "/project_dir/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules node-server-data-volumes-app:volumes
```

One important note in this case, since the whole `/app` directory is read-only, the other directories inside it will also be read-only. So I can declare an anonymous volume for `/app/feedback` to allow the container to write to that directory, as it's more specific, Docker will merge the volumes and the anonymous volume will take precedence over the read-only bind mount for `/app`.

## Docker Volume Commands

Some useful commands to manage volumes:

- `docker volume ls`: List all volumes.
- `docker volume create {VOLUME_NAME}`: Create a new volume (though Docker creates those automatically when using the -v flag).
- `docker volume inspect {VOLUME_NAME}`: Inspect a volume to see its details, like the mount point on the host filesystem.
- `docker volume rm {VOLUME_NAME}`: Remove a volume. This will only work if the volume is not in use by any container.
- `docker volume prune`: Remove all unused volumes. This is useful to clean up volumes that are no longer needed.

## Environment Variables

To set environment variables I can declare them in the Dockerfile using the `ENV` command and reference them both in the Dockerfile and in the application code (see PORT in server.js).

Usage example:

```shell
docker run -d --rm --env PORT=8000 --name node-server-app -p 8080:8000 -v feedback-v:/app/feedback -v ".:/app:ro" -v /app/node_modules -v /app/temp node-server-data-volumes-app:env
```

I could also pass the environment variables using an `.env` file.

```shell
docker run -d --rm --env-file .env --name node-server-app -p 8080:8000 -v feedback-v:/app/feedback -v ".:/app:ro" -v /app/node_modules -v /app/temp node-server-data-volumes-app:env
```

## Arguments

Arguments are used to pass values to the Dockerfile at build time. They are defined using the `ARG` command in the Dockerfile and can be used to customize the build process.

```shell
docker build -t node-server-data-volumes-app:dev --build-arg DEFAULT_PORT=8000 .
```
