#  Student Registration Website.

---

##  Frontend -
Node.js and npm must be installed.

---
##  Backend -
Java Development Kit (JDK 17 or higher) must be installed.

Maven must be installed.

---
##  Database -
Install MariaDB.

---
##  Before starting, make sure:-

1. RDS (Relational Database Service) is created.

2. EC2 Instance is launched.
 
3. Docker is installed.
 
4. MySQL Client is installed.
   
---

##  Steps and Commands:


## 1. RDS (Relational Database Service) -

. Create Database

. Standard create 

. MariaDB

. Username - (admin) - Ex.

. Password - (redhat123) - Ex.

. Create Database

## 2. EC2 (Elastic Compute Cloud) -

. Launch instance 

. Connect


### 1. Switch to root user
```shell
sudo -i
```

### 2. Update the instance
```shell
apt update
```
### 3. Install MySQL client
```sh
apt install mysql-client -y
```

### 4. Mysql
```sh
mysql -h (endpoint) -u (username) -p
```
Enter password (password)

#### RDS Database Endpoint copy & paste
#### Example: mysql -h database-1.ca9eie2mihs7.us-east-1.rds.amazonaws.com -u admin -p
#### Example: redhat123

```sh
CREATE DATABASE student_db;
```
```sh
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```
```sh
show databases;
```
```sh
exit;
```
#### Example: GRANT ALL PRIVILEGES ON springbackend.* TO 'admin'@'localhost' IDENTIFIED BY 'redhat123';


### 5. Install Docker
```sh
apt install docker.io -y
```

### 6. Clone the GitHub Repository
```sh
git clone <GitHub_Repository_Link>
```

# Example: git clone https://github.com/username/student-registration.git

cd <GitHub_Repository_Name>/backend
cp src/main/resources/application.properties .

# Write application.properties =
nano application.properties
    server.port=8080
    spring.datasource.url=jdbc:mariadb://(database endpoint paste):3306/student_db 
    spring.datasource.username=(database username)
    spring.datasource.password=(database password)
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
# Then ctrl s+x

## 7. Backend =

# Write Backend Dockerfile =
nano dockerfile
    FROM maven:3.8.3-openjdk-17
    COPY . /opt
    WORKDIR /opt
    RUN rm -rf src/main/resources/application.properties
    RUN cp -rf application.properties src/main/resources
    RUN mvn clean package
    WORKDIR /opt/target
    EXPOSE 8080
    CMD ["java","-jar","student-registration-backend-0.0.1-SNAPSHOT.jar"]

# Build Backend Dockerfile
docker build . -t backend :v1

# Check docker images
docker images

# Docker Container run 
docker run -d -p 8080:8080 backend :v1

# Check docker container
docker ps

## 8. Frontend =

cd ../frontend/
ls

# Write .env file =
nano .env

# Example: (Paste EC2 public IP)

# Write Frontend dockerfile =
nano dockerfile
    FROM node:25-alpine3.21
    COPY . /opt
    WORKDIR /opt
    RUN apk update 
    RUN apk add apache2
    RUN npm install
    RUN npm run build
    RUN cp -rf dist/* /var/www/localhost/htdocs/
    EXPOSE 80
    CMD ["httpd","-D","FOREGROUND"]


# Build Frontend Dockerfile
docker build . -t frontend :v1

# Check docker images
docker images

# Docker Container run 
docker run -d -p 80:80 frontend :v1

# Check docker container
docker ps

```


## 3. Access website:

Open your browser and go to

```
http://<EC2-Public-IP>
```
