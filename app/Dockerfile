FROM node:alpine

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

RUN npm install -g bower gulp

WORKDIR /var/www/app

# RUN npm install

# RUN bower install --allow-root

# CMD ["gulp"]