 
# Flask App with MySQL Docker Setup

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Clone this repository (if you haven't already):

   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

2. Navigate to the project directory:

   ```bash
   cd your-repo-name
   ```

3. Create a `.env` file in the project directory to store your MySQL environment variables:

   ```bash
   touch .env
   ```

4. Open the `.env` file and add your MySQL configuration:

   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```
## To run this two-tier application using  without docker-compose

- First create a docker image from Dockerfile
```bash
sudo chmod 666 /var/run/docker.sock
docker build -t flaskapp-2tier .
docker images
```
- Now Run the created docker Container with the help of Docker images with Mentioned Port
```bash
docker run -d -p 5000:5000 flaskapp-2tier:latest
```
-Run Locally image container
```bash
docker run  -t -p 5000:5000   --name  flaskapp flaskapp-2tier:latest
docker exec -it flaskapp
docker ps
```
-Now Access the app with the help of the Public IP of Ec2 Instance with port no 
```bash 
http://<Public_IP>:5000
```
-We need to create a MySQL image & Container.
```bash 
docker run -d -p 3306:3306 --name mysql  mysql:5.7
---
docker ps
--- 
docker rmi $(docker images -q)
```
- We need to create a docker network 2tier to connect MySQL Container and Flask app Container.
```bash
docker network create twotier
docker network ls
```
Stop previous running containers, before creating new containers.
```bash
docker ps -a
docker rm $(docker ps -a -q)
docker kill <container-id-1> <container-id-2>
docker rm <container-id-1> <container-id-2>
```
- Attach both the containers in the same network, so that they can communicate with each other and attached env vars 

i) MySQL container 
```bash
docker run -d -p 3360:3360 --name mysql --network=twotier -e MYSQL_DATABASE=testdb -e MYSQL_USER=admin -e MYSQL_ROOT_PASSWORD=<yourpassword> -e MYSQL_PASSWORD=<yourpassword> mysql:5.7
```
-check if the mysql container has been added to the new network created
```bash
docker inspect <network name>
```
ii) Backend container
```bash
docker run -d -p 5000:5000  --name flask-app --network=twotier -e MYSQL_HOST=mysql -e MYSQL_DB=testdb -e MYSQL_USER=admin -e MYSQL_PASSWORD=<yourpassword> flaskapp-2tier:latest

docker inspect <network name>
```
- creat database table in mysql container
```bash 
docker exec -it <container id> bash
mysql -u root -p
show databases;
use myDb;

CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);

```

# Push Your Docker Image To DockerHub
-Login to your docker hub account.
```bash
docker login
  Username: <dockerhub_username>
  Password: <dockerhub_password>
```
- Now tag your docker image with your dockerhub username.
```bash 
  docker images
  docker tag flaskapp-2tier:latest nusratdev/2-tier-flask-app:latest
  docker images
```
  - Push your tagged docker image to your docker hub account.
```bash
    docker push nusratdev/2-tier-flask-app:latest
```
## Usage In LocalHost

1. Start the containers using Docker Compose:

```bash
   docker-compose up -d --build
```

2. Access the Flask app in your web browser:

   - Backend: http://localhost:5000

3. Create the `messages` table in your MySQL database:

   - Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
   
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```

## Cleaning Up

To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:

```bash
docker-compose down
```


