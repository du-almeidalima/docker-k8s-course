FROM node:slim

# This is useful to set where the commands like COPY and RUN will be executed.
# As I copied the files to /app so I set the working directory to /app.
WORKDIR /app

# The first parameter relative to the where the Dockerfile is located and the second parameter is the path inside the container
# Since the WORKDIR is set to /app, the second parameter is relative to /app
# Copying only to allow Docker to cache the npm install layer.
COPY package.json ./

# I could also use absolute path like this:
# COPY . /app

# This is the command that will be executed when the image is being built.
RUN npm install

# Copying the rest of the files to the image.
COPY . ./

# Containers are isolated from the host system and other containers. In order to access the application running inside 
# the container, we need to expose a port.
# However, this does not publish the port. It just tells Docker that the application inside the container will be using this port.
# This is useful for documentation purposes and for other containers that might need to communicate with this container.
# But to actually publish the port, we need to use the -p flag when running the container.
EXPOSE 80

# Doing this would wrong as in the Dockerfile I'm just creating the image.
#RUN node server.js
# This is the command that will be executed when the container is run/started.
CMD ["node", "server.js"]