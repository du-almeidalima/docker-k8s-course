# 1 - Getting Started

This lesson introduced showed how to create a Dockerfile for this demo Node.js app and the first Docker commands:
- `docker build`
- `docker run`

I also learned how to map a port from the host to the container.
- `docker run -p 3000:3000 nodeapp:1`

To visualize the running containers, I can use the following command:
- `docker ps`

And to stop a container, I can use the following command:
- `docker stop <container_id>`

## Observations
- The `docker build` command creates an image from a Dockerfile.
  - I can give this image a name with the `-t` flag. I used the following command `docker build . -t nodeapp:1`