FROM nginx:stable-alpine

WORKDIR /etc/nginx/conf.d

COPY ./laravel_react_docker/nginx/default.conf .

RUN mv default.conf default.conf

WORKDIR /var/www/html

COPY ./projects .