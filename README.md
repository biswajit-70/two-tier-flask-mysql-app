# Two-Tier Docker Application

This is a **two-tier application** consisting of:

1. **Backend**: A Flask application.
2. **Database**: MySQL 5.7.

The application uses **Docker** to run the backend and database in separate containers connected via a custom Docker network.

---

## Prerequisites

- Docker installed on your machine.
- Basic knowledge of Docker commands.
- Port 5000 (for Flask) and 3306 (for MySQL) available on your system.

---

## Steps to Run

### 1. Build the Flask Docker Image

```bash
docker build -t flaskapp .
```
### 2. Create a Docker Network
```bash
docker network create flasktier
```
- This network allows the backend and database containers to communicate.
### 3. Run the MySQL Container
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=flasktier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```
#### Explanation: 
- --name mysql: Names the container mysql.

- -v mysql-data:/var/lib/mysql: Persists database data.

- --network=twotier: Connects container to twotier network.

- -e MYSQL_DATABASE=mydb: Creates a database named mydb.

- -e MYSQL_ROOT_PASSWORD=admin: Sets the root password.

- -p 3306:3306: Maps MySQL port to host.
### 4. Run the Flask Backend Container
```bash
docker run -d \
    --name flaskapp \
    --network=flasktier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest

```
#### Explanation:

- --name flaskapp: Names the container flaskapp.

- --network=twotier: Connects container to the twotier network.

- Environment variables connect Flask to MySQL:

- MYSQL_HOST=mysql

- MYSQL_USER=root

- MYSQL_PASSWORD=admin

- MYSQL_DB=mydb

- -p 5000:5000: Exposes Flask app on port 5000.
### 5. Build the Flask Docker Image
```bash
docker build -t flaskapp .
```
## Access the Application

- Flask App: Open http://localhost:5000 in your browser.

- MySQL: Connect using any MySQL client at localhost:3306 with username root and password admin


## Stopping and Removing Containers

```bash
  docker stop flaskapp mysql
  docker rm flaskapp mysql
  docker network rm twotier

```


### Notes

- Ensure ports 5000 and 3306 are not used by other services.

- Data in MySQL persists in Docker volume mysql-data.

- Containers communicate through the custom flasktier network.


## Authors

- Biswajit pattanaik
- gmail : biswajitpattanaik453@gmail.com


![Logo](https://media.licdn.com/dms/image/v2/D4D12AQFWFLeRgjzEdA/article-cover_image-shrink_423_752/article-cover_image-shrink_423_752/0/1674452473282?e=1760572800&v=beta&t=IilyXBtL_Vzlexld_OyDwAE3iRXmPbO4X9IsnCtukbI)  

![Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Docker_%28container_engine%29_logo.svg/915px-Docker_%28container_engine%29_logo.svg.png?20161017201350)

![logo](https://www.mysql.com/common/logos/logo-mysql-170x115.png)

