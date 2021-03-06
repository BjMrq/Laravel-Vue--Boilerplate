<p align="center"><img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400"></p>


## Project

This project contain the required configurations to get up and running with a Laravel/Vue app inside Docker containers.

## Prerequisites

Having Docker and Docker-compose on your machine.

## Prepare environement

Clone the project repository
```
git clone git@github.com:cominityio/poject_name.git
```
OPTIONAL: If you are cloning the repo from the boilerplate don't forget to remove the .git history
```
rm -rf .git
git init
```
Create a new .env file similar to .env.example  
APP_KEY= should stay blank for now

Choose your databse name / user name / password (there will be share in all your containers)  
Add the following (notice that the host is referenced as db, it will be used by Docker to establish the connection)
```
DB_CONNECTION=mysql
# Developing with docker
DB_HOST=db
# Developing without docker
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_db_name
DB_USERNAME=your_db_user
DB_PASSWORD=your_db_password
```
You can now enter the following commands (the build flag is only needed the first time or when you install a new dependency)
```
docker-compose up --build
```

:warning: Either install new dependencies inside the app container or if you install from host rebuilt the containers :warning:  

Before accessing the project you need to enter the app container
```
docker-compose exec app bash
```
Install composer dependencies
```
composer install
```
Generate a new key for your app
```
php artisan key:generate
```
Once the key has been generated you will need to shut down and compose back up your containers to load the new environement variables  

To do so, run the following commands (notice that we don't need the --build flag since since we didn't change the dependencies)
```
docker-compose down
docker-compose up
```
Then open bash into the app container again and produce a mix manifest for Vue using Yarn
```
docker-compose exec app bash
yarn dev
```
You can now run the watch command that will enable hot reloading
```
yarn watch
```
You can now acces the project at http://localhost:80

You can also acces phpmyadmin at http://localhost:8080 

To shutdown the containers run
```
docker-compose down
```
At the end of the project, to clean your system of all containers or db volumes run
```
docker system prune -a
```

## Debuging

:warning: Either install new dependencies inside the app container or if you install from host rebuilt the containers :warning:  

To enter in the app container run
```
docker-compose exec bash
```
If you already have mysql or apache running in your computer you'll need to free the ports, run
```
sudo systemctl stop mysql
```
```
sudo /etc/init.d/apache2 stop
```
If you are missing privileges you can run
```
sudo chown -R $USER:$USER ~/path/to/project
```
If you are having trouble deleting the node_modules repository run from your host
```
sudo rm -rf node_modules
```
