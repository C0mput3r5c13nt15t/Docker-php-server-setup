# Deploy your php application in minutes!
This is the template for an docker php apache server. It's purpose is to help you deploy your php application faster and more easily.

# How to use
To use this repository you first need to [install docker-compose](https://docs.docker.com/compose/install/).

1. Clone this repository into your home directory
2. Put your php application into the src file
3. Run ```docker-compose up -d``` to deploy the conatiner
4. Voila your php application should now be accessable over http://yourdomainOrIP

# Connect to the Database
The conatiner comes with a postgres db, which makes it ARM compatible! To connect your application to it edit the docker-compose.yml file.

```yml
...
    environment:
      - POSTGRES_USER=YOUR_DB_USER
      - POSTGRES_PASSWORD=YOUR_DB_PASSWORD
      - POSTGRES_DB=YOUR_DEFAULT_DB
...
```

Make sure you have set the credentials in your application accordingly!
