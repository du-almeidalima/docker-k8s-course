# Use official Node.js image for build stage
FROM node:slim AS build

WORKDIR /app

COPY package.json package-lock.json /app/
RUN npm install

COPY . .
RUN npm run build

# Use Nginx to serve the static files
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

# Optional: Copy custom nginx config if needed
# COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
