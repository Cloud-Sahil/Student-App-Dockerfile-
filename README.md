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
USE student_db;
```
```sh
CREATE TABLE `students` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `course` varchar(255) DEFAULT NULL,
  `student_class` varchar(255) DEFAULT NULL,
  `percentage` double DEFAULT NULL,
  `branch` varchar(255) DEFAULT NULL,
  `mobile_number` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=80 DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
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

#### Example: git clone https://github.com/username/student-registration.git
```sh
cd <GitHub_Repository_Name>/backend
```
```sh
cp src/main/resources/application.properties .
```
### Write application.properties =
```sh
nano application.properties
```
```sh
server.port=8080
spring.datasource.url=jdbc:mariadb://(database endpoint paste):3306/student_db 
spring.datasource.username=(database username)
spring.datasource.password=(database password)
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```
### Then ctrl s+x

### 7. Backend =

### Write Backend Dockerfile =
```sh
nano dockerfile
```
```sh
FROM maven:3.8.3-openjdk-17
COPY . /opt
WORKDIR /opt
RUN rm -rf src/main/resources/application.properties
RUN cp -rf application.properties src/main/resources
RUN mvn clean package
WORKDIR /opt/target
EXPOSE 8080
CMD ["java","-jar","student-registration-backend-0.0.1-SNAPSHOT.jar"]
```
### Build Backend Dockerfile
```sh
docker build . -t backend :v1
```

### Check docker images
```sh
docker images
```
### Docker Container run 
```sh
docker run -d -p 8080:8080 backend :v1
```
### Check docker container
```sh
docker ps
```
### 8. Frontend =
```sh
cd ../frontend/
```
```sh
ls
```

### Write .env file =
```sh
nano .env
```
#### Example: (Paste EC2 public IP)

### Write Frontend dockerfile =
```sh
nano dockerfile
```
```sh
ROM node:25-alpine3.21
COPY . /opt
WORKDIR /opt
RUN apk update 
RUN apk add apache2
RUN npm install
RUN npm run build
RUN cp -rf dist/* /var/www/localhost/htdocs/
EXPOSE 80
CMD ["httpd","-D","FOREGROUND"]
```

### Build Frontend Dockerfile
```sh
docker build . -t frontend :v1
```
### Check docker images
```sh
docker images
```
### Docker Container run 
```sh
docker run -d -p 80:80 frontend :v1
```
### Check docker container
```sh
docker ps
```


## 3. Access website:

Open your browser and go to

```
http://<EC2-Public-IP>
```
