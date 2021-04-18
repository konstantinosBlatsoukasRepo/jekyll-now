---
layout: post
title: JPA/Hibernate setup with sample database
---

How to run locally a mysql databse and connect the database to a spring application.

What you will need:

- [docker](https://www.docker.com/products/docker-desktop) installed
- a [jdk](https://www.oracle.com/java/technologies/javase-downloads.html) (java development kit)  
- an IDE of your choice (I am using [Intellij](https://www.jetbrains.com/idea/download/?fromIDE=#section=windows))
- a database client (I am using [DBeaver](https://dbeaver.io/download/))

## Set up

1. Start a mysql server with a sample database

  I used the this [image](https://hub.docker.com/r/genschsa/mysql-employees) from docker hub.
  By running the following command, you will get a mysql database and a sample employees

  ```sh
  docker run -d \
    --name mysql-employees \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=college \
    genschsa/mysql-employees
  ```
 
 Verify that the container is up and running by typping in a terminal ``` docker container ls ```.
 You should see something like this:
 
  ```sh
  CONTAINER ID   IMAGE                      COMMAND                  CREATED      STATUS       PORTS                               NAMES
  ab35057ee13c   genschsa/mysql-employees   "docker-entrypoint.s…"   7 days ago   Up 2 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-employees
  ```
  
2. Start a new java project

- navigate to [spring initialzr](https://start.spring.io/)
- add as dependencies the *Spring JPA* and *MySQL driver*

3. at application.properties file add the following lines

```sh
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/employees
spring.datasource.username=root
spring.datasource.password=college

# show the queries that JPA generates
spring.jpa.show-sql=true
```

4. Finally, try top start the application (make sure that the container is up), 
   if you see no error that means that you have successfully connected the app 
   to the MySQL database 
