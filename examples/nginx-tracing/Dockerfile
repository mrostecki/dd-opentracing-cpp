# Builds and runs a simple nginx server, traced by Datadog
FROM ubuntu:18.04

ARG NGINX_VERSION=1.14.1
ARG DATADOG_PLUGIN_VERSION=0.3.7

RUN apt-get update && \
  apt-get install -y git gnupg wget tar

# Install nginx
RUN wget https://nginx.org/keys/nginx_signing.key && \
  apt-key add nginx_signing.key && \
  echo deb https://nginx.org/packages/ubuntu/ bionic nginx >> /etc/apt/sources.list && \
  echo deb-src https://nginx.org/packages/ubuntu/ bionic nginx >> /etc/apt/sources.list && \
  apt-get update && \
  apt-get install nginx=${NGINX_VERSION}-1~bionic
# Configure nginx
COPY ./examples/nginx-tracing/nginx.conf /etc/nginx/nginx.conf
COPY ./examples/nginx-tracing/dd-config.json /etc/dd-config.json
RUN mkdir -p /var/www/
COPY ./examples/nginx-tracing/index.html /var/www/index.html

# Install Datadog module
ADD https://github.com/DataDog/dd-opentracing-cpp/releases/download/v${DATADOG_PLUGIN_VERSION}/linux-amd64-nginx-${NGINX_VERSION}-ngx_http_module.so.tgz linux-amd64-nginx-${NGINX_VERSION}-ngx_http_module.so.tgz
RUN tar zxf linux-amd64-nginx-${NGINX_VERSION}-ngx_http_module.so.tgz -C /usr/lib/nginx/modules

# Test nginx config.
RUN nginx -t

EXPOSE 80
CMD [ "nginx", "-g", "daemon off;"]
