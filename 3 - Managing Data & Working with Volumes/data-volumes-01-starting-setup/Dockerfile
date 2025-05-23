FROM node:slim

WORKDIR /app

COPY ./package.json .

RUN npm install

COPY . .

EXPOSE 80

# The path INSIDE of the container, that should be mapped to a directory on the host machine.
# The directory of the host machine is not actually provided here, instead, Docker is controlling where it's placed.
# This creates an anonymous volume, managed by Docker. It only exists while the container also exists, which doesn't
# help much in our use case, which is persist the files between containers.
# VOLUME ["/app/feedback"]

CMD ["node", "server.js"]