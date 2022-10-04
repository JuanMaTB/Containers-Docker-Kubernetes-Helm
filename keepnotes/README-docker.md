## docker-flask-template
This is a basic docker container template, containing a Flask app and a Mysql database both registered as services in a docker-compose.

Since both containers belong to the same default network, Flask app is able to see the database.

In addition, after creating the images, both images are pushed into dockerhub, so they are used in kubernetes solution.

Given the docker-compose file, I will use kompose in order to export all the kubernetes manifests needed in to install and scale up the containers previously mentioned.

## Building App

### requeriments:
Flask
mysql-connector


### Network create
Docker network create spider

### building python alpine
```docker run --name notes -d -v $(pwd)/app:/app -v $(pwd)/req.txt:/req.txt --env FLASK_APP=app.py --env FLASK_ENV=development --env FLASK_RUN_HOST=0.0.0.0 --env FLASK_RUN_PORT=5000 --network spider -p 5000:5000 python:3.11.0rc2-alpine3.16 sh -c "apk add gcc musl-dev linux-headers curl; pip install -r req.txt; flask run"```

## Building database
imagen mysql oficial latest y para tener dentro la tabla que necesitamos usaremos el archivo keepnotes.sql que esta en el repositorio. para usarlo en bash, podriamos usar el siguiente.
#!/bin/bash

mysql --password=$MYSQL_ROOT_PASSWORD -u root <<EOS


CREATE DATABASE IF NOT EXISTS `keepnotes` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `keepnotes`;

CREATE TABLE IF NOT EXISTS `users` (
	`id` int(10) NOT NULL AUTO_INCREMENT,
    `email` varchar(100) NOT NULL,
    `password` varchar(255) NOT NULL,
    `name` varchar(100) NOT NULL,
    PRIMARY KEY(`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `notes` (
	`noteid` int(10) NOT NULL AUTO_INCREMENT,
    `content` varchar(1000) NOT NULL,
    `userid` int(10) NOT NULL,
    PRIMARY KEY(`noteid`),
    CONSTRAINT FOREIGN KEY(`userid`) REFERENCES users(`id`)
    ON UPDATE CASCADE
    ON DELETE CASCADE
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

EOS

Now its time to Docker build and docker-compose build.
                                                    

### In order to startup the containers, please run:

```docker-compose -f docker-compose.yaml up --no-start```
```docker-compose -f docker-compose.yaml start```
### and to stop the containers:
```docker-compose -f docker-compose.yaml stop```
### Stop and remove containers 
```docker-compose -f docker-compose.yaml down ```   
						
## Checking
						    
Lets use ```Docker ps``` command to see our dockers running and lets use ```Docker exec it <docker name> bash``` to enter in our docker and use ```ls -la /dev/stderr``` to check errors and ```ls -la /dev/stdout``` to check outputs.
						    

