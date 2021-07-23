# Deploy your php application in minutes!
This is the template for an docker php apache server. It's purpose is to help you deploy your php application faster and more easily.

# How to use
To use this repository you first need to [install docker-compose](https://docs.docker.com/compose/install/).

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

# Issues
If you encounter an issue or parts of this tutorial are outdated please don't hesitate to contact me.
