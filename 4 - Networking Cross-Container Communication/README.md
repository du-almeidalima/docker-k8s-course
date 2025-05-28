# 4 - Networking Cross-Container Communication

## WWW

Out of the box, Docker containers can access the internet, like calling API endpoints or downloading files.

## Host Communication

Containers can also communicate with services/applications running on the host machine. However, instead of using the host's IP address or `localhost`, you can use the special DNS name `host.docker.internal` to refer to the host machine from within a container.

## Cross-Container Communication

In this example, a Node.js application running in one container will communicate with a MongoDB database running in another container.

> Notice that no port mapping is needed for the MongoDB container, as it will be only accessed by the Node.js application.

### First Approach: Using MongDB's Internal IP Address

This approach involves using the internal IP address of the MongoDB container to connect from the Node.js application.

To create the MongoDB container and get its IP address (Networks.bridge.IPAddress):

```shell
docker run -d --name mongodb mongo
```

```shell
docker container inspect mongo
```

Before creating the Node.js container, it's necessary to know the MongoDB container's IP address and add it to the Node.js mongo connection.

To create the Node.js container:

```shell
docker build . -t favorites-node-app
```

```shell
docker run -d --name favorites-node-app -p 3000:3000 favorites-node-app
```

### Second Approach: Using Docker Networks

This approach involves creating a Docker network and connecting both the MongoDB and Node.js containers to that network. This allows them to communicate using their container names as hostnames.

With this approach, I can create a Docker network and connect both containers to it when running them.

To create a Docker network:

```shell
docker network create favorites-net
```

> Unlike volumes, networks are not automatically created when running a container. You need to create them explicitly.

```shell
docker run --network favorites-net -d --name mongodb mongo
```

```shell
docker run --network favorites-net -d --name favorites-node-app -p 3000:3000 favorites-node-app
```

Just like before with the host connection, Docker translated the `host.docker.internal` to the host's IP address. With networks, Docker translates the container name to its internal IP address. So in the Node.js application, I can use `mongodb` as the hostname to connect to the MongoDB container.