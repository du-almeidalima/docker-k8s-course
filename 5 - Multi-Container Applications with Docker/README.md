# Multi-Container Applications with Docker

This module of the course builds a multi-container application set up using Docker. It includes the following components:

- MongoDB
- Node.js Application
- React SPA Application

For that to happen, a docker network is created to allow communication between the containers. The Node.js application connects to the MongoDB database, and the React SPA application communicates with the Node.js application.

> **Note:** The Front-End application can't use the network name to connect to the Node.js application. Because the React SPA is execute on a the client browser, that is outside the Docker network. Instead, the React SPA uses the localhost address with the published port of the Node.js application.
> The --network on the React SPA is not really necessary, but it is included for consistency.

## Database Setup

Creating the cluster Network

```shell
docker network create goals-app-net
```

```shell
docker run -d \
  --name goals-mongodb \
  -p 27017:27017 \
  -v goals_mongodb_data:/data/db \
  --network goals-app-net \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=admin \
  mongo:latest
```

## Node.js Application Setup

```shell
docker build . -t goals-node-app
```

```shell
docker run -d --name goals-node-app -p 8000:80 -v goals_node_app_logs:/app/logs --network goals-app-net goals-node-app:latest
```

## React SPA Application Setup

```shell
docker build -t goals-react-app .
```

```shell
docker run -d --name goals-react-app -p 3000:80 --network goals-app-net goals-react-app:latest
```