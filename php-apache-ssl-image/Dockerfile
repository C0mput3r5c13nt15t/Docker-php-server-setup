FROM php:7.4-apache

# Install the ssl-cert package
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y ssl-cert \
 && rm -r /var/lib/apt/lists/*

# Enable ths ssl module
RUN a2enmod ssl
