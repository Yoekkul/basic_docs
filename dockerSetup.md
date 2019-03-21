# Installing & running sql on docker



## Installing needed packages

1. Docker engine which enables us to create containers

`sudo apt-get install docker`
2. Docker-compose which allows us to setup the containers as we like

`sudo apt-get install docker-compose`

## Create dockerfile
A `docker-compose.yml` file enables us to specify how we want to setup the docker
containers.

```
version: "3"
services:
        dmdb-db:
                image: postgres
                container_name: dmdb-db
                environment:
                        POSTGRES_USER: theuser
                        POSTGRESS_PASSWORD: thepassword
                volumes:
                        - /var/lib/postgresql/data
                ports:
                        - "5431:5432"
        dmdb-admin:
                image: adminer
                container_name: dmdb-admin
                environment:
                        ADMINER_DEFAULT_SERVER: dmdb-db
                ports:
                        - "8080:8080"
                depends_on:
                        - dmdb-db
```

TODO insert explaination for lines

## Starting the docker container

In the file where we saved our `docker-compose.yml` file we need to run the command `sudo docker-compose up`. This will start the Docker environment. To test that the setup occurred correctly we can now open any webbrowser and enter the address `localhost:8080`

To close the container disconnect all connected services (eg: pgadmin) and press `ctrl+c`

## Loging into the db
### From the web interface
Select Postgresql drom the system dropdown, enter theuser name and thepassword
You will be greeted by an interface on which yo will be able to interact with your databases

### From pgadmin
Select **Add connection to the server** (The top left power cable plug shaped button). Enter a name you like `0.0.0.0` as host and `5431` as the port. Don't forget to set you username and password

## Interfacing with the docker container
Listing all currently active docker containers (including their Container ID !!): `sudo docker container ls`



## Loading data into the docker container
`sudo docker cp [source_directory/file] [docker container id (even a few letters are enough)]:/var/lib/postgresql/data
`
It is important to save the data inside the docker container in the above directory since we defined this as a permanent directory in the dockerfile (which thus isn't deleted each time the container shuts down)
