# Deploy your php application in minutes!
This is the template for an docker php apache server. It's purpose is to help you deploy your php application faster and more easily.

# How to use
To use this repository you first need to [install docker-compose](https://docs.docker.com/compose/install/) (Watch out! It's different [for raspberry pi](https://dev.to/rohansawant/installing-docker-and-docker-compose-on-the-raspberry-pi-in-5-simple-steps-3mgl)).

1. Make a folder in your home directory and clone this repository into it
2. Put your php application into the src file
3. Run ```docker-compose up -d --build``` to deploy the conatiner
4. Voila your php application should now be accessable over http(s)://yourdomainOrIP

# Preconfigured with ssl
If you did everything according to the instructions your app should already have a https connection configured with a self signed ssl certificate. This is already a great start but you may want to learn how to [configure a ssl certificate with let's encrypt certbot](https://letsencrypt.org/de/getting-started/).

# Connect your app to the database
The conatiner comes with a postgres db, which makes it ARM compatible! To connect your application to it edit the docker-compose.yml file and configure your app accordingly.

```yml
...
    environment:
      - POSTGRES_USER=YOUR_DB_USER
      - POSTGRES_PASSWORD=YOUR_DB_PASSWORD
      - POSTGRES_DB=YOUR_DEFAULT_DB
...
```

# Further configuration
In the docker-compose.yml file you should replace "yourapp" with your actual app name and also edit the vhost configuration to correct server name and ths admin contact formular.

# Deploy a Laravel project
To deploy a laravel you will first need to complete the steps above and then continue with this section.

1. Append the following to the docker-compose.yml file
```Dockerfile
...

# Install composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Install git
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y git

# Get your laravel application
RUN git clone https://github.com/yourusername/yourrepo.git .

# Install Laravel and configure permissions
RUN composer install --no-dev
RUN chown -R :www-data /var/www/html && chmod -R 775 /var/www/html/storage && chmod -R 775 /var/www/laravel/bootstrap/cache

# Copy the .env
COPY .env /var/www/html/.env

# Generate app key
RUN php artisan key:generate
```
2. Create an .env file in the app-apache-ssl-image folder (for details on what to put in there see [the ultimate laravel guide](https://devmarketer.io/learn/deploy-laravel-5-app-lemp-stack-ubuntu-nginx/))
3. Change you vhost.conf file in docker/php-apache to look something like this
```conf
Listen 8080
Listen 8443

<VirtualHost *:8080>
  ServerName mysite.example.com
  ServerAdmin webmaster@localhost

  DocumentRoot "/var/www/html/public/"
  <Directory "/var/www/html/public/">
    Options Indexes FollowSymLinks
    AllowOverride All
    allow from all
    Require all granted
    DirectoryIndex index.php index.html
  </Directory>

  # DocumentRoot /var/www/html/public/
  # Redirect / https://mysite.example.com/
</VirtualHost>

<VirtualHost *:8443>
  ServerName mysite.example.com
  ServerAdmin webmaster@localhost

  SSLEngine on
  SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
  SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key

  DocumentRoot "/var/www/html/public/"
  <Directory "/var/www/html/public/">
    Options Indexes FollowSymLinks
    AllowOverride All
    allow from all
    Require all granted
    DirectoryIndex index.php index.html
  </Directory>
</VirtualHost>
```
5. run ```docker-compose up -d --build```

# Links
1. [The absolutely GREATEST guide for deploying laravel](https://devmarketer.io/learn/deploy-laravel-5-app-lemp-stack-ubuntu-nginx/)

# Issues
If you encounter an issue or parts of this tutorial are outdated please don't hesitate to contact me.
