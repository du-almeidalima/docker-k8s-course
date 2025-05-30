FROM node:slim

WORKDIR /app

COPY ./package.json ./package-lock.json /app/

RUN npm install

EXPOSE 80

ENV MONG_DB_URL='mongodb://goals-mongodb:27017/course-goals'
ENV MONG_DB_USER='admin'
ENV MONG_DB_PASSWORD='admin'

COPY . .

CMD ["node", "app.js"]

