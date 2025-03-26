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
Access the application at: [http://localhost:8080](http://localhost:8080)

## Run the Application with Docker (Recommended)
### Build the Docker Image
```sh
docker build -t ultimate-cicd-pipeline:v1 .
```
### Run the Docker Container
```sh
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```
Access the application at: [http://<ip-address>:8010](http://<ip-address>:8010)

---

# Next Steps: Configure a SonarQube Server Locally
SonarQube is used for static code analysis. Follow these steps to set it up.

## System Requirements
### Software:
- Java 17+ (Oracle JDK, OpenJDK, or AdoptOpenJDK)

### Hardware Recommendations:
- Minimum 2 GB RAM
- 2 CPU cores

## Install and Configure SonarQube
```sh
sudo apt update && sudo apt install unzip -y
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
unzip sonarqube-10.4.1.88267.zip
sudo mv sonarqube-10.4.1.88267 /opt/sonarqube
sudo chown -R sonarqube:sonarqube /opt/sonarqube
sudo chmod -R 775 /opt/sonarqube
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start
```

Access the SonarQube Server at: [http://<ip-address>:9000](http://<ip-address>:9000)

---

## Hurray!! 🎉
You have successfully set up and run the Spring Boot application and SonarQube server!

