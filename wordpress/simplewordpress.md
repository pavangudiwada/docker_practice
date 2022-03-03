# Creating a Simple wordpress site using Docker

## Resources used: 

[How to Easily Setup Wordpress Using Docker](https://medium.com/@habibridho/how-to-easily-setup-wordpress-using-docker-a798081dc577)

## What will we use?
* Docker
* Wordpress
* Mysql 
* Docker network 

## Steps: 

1. We create a MySql image and then exec into it to create a database.

2. We then create a wordpress image and expose internal port 80 to external 8080.

3. Wordpress cannot detect the MySql database so we create a new network and add both the containers to the network.

4. We add the database details to the Wordpress site and done!

## Step 1:

Use `docker run --name wp-backend -e MYSQL_ROOT_PASSWORD=12345 -d mysql:latest`

* -e -> environment variable
* -d -> detached mode

Wait for a few minutes then run `docker exec -it wp-backend mysql -u root -p` 

* exec -> Execute a command
* -it -> Interactive terminal of 
* mysql -u root -p -> The command that is to be run inside the Mysql container
    * -u -> username 
    * -p -> asks for a password

Once inside run `create database wordpress;` to create a database for us to use.

Use `exit` to exit the container.

## Step 2:

Run `docker run --name wp-frontend -p 8080:80 wordpress` 

* -p -> ports
* 8080:80 -> Attach 8080 of host machine to 80 of wordpress container.

## Step 3:

Our wp-frontend(wordpress container) cannot access the wp-backend(Mysql container) so we create a network and add both of them to it.

Use `docker network create --attachable wp-network` 

* --attachable -> Lets you add containers manually to this network.

Now we should connect both **wp-frontend** and **wp-backend** containers to the **wp-network** network.


```
docker network connect wp-network wp-frontend
docker network connect wp-network wp-backend
```

Go to localhost:8080 and add the details

Database Name -> wordpress

Username -> root	

Password -> the password you set

Database Host -> wp-backend	
	


### Tips: 

Use `docker stop containerID` to stop the container before you delete it. 

`docker rm ContainerID` to remove the image. 

You can use `docker rm ContainerID -f` to directly stop and delete the container.
