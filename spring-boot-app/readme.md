# Spring Boot Web Application

This is a simple Spring Boot-based Java web application built using Maven. It follows the MVC architecture, where the controller returns a page with `title` and `message` attributes to the view.

## Prerequisites
- Java 11 or later (Recommended: Java 17+)
- Maven
- Docker (Optional, but recommended for easier execution)
- Git

## Clone the Repository
```sh
git clone https://github.com/chidhvilaskacham/ultimate-cicd.git
cd spring-boot-app
```

## Build the Application
Run the following command to build the application and generate artifacts:
```sh
mvn clean package
```
The generated artifact will be available in the `target` directory.

## Run the Application Locally
### Using Java (Requires Java 11+)
Execute the following command:
```sh
java -jar target/spring-boot-web.jar
```
Access the application at port 8080

## Run the Application with Docker (Recommended)
### Build the Docker Image
```sh
docker build -t ultimate-cicd-pipeline:v1 .
```
### Run the Docker Container
```sh
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```
Access the application at port 8010

---


## Hurray!! 🎉
You have successfully set up and run the Spring Boot application!

