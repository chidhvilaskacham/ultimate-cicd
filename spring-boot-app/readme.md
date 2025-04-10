# Spring Boot Web Application

This is a simple Spring Boot-based Java web application built using Maven. The application follows the **MVC** architecture, where the controller returns a page with `title` and `message` attributes to the view.

## Prerequisites

Before you can run this application, ensure you have the following installed:

- **Java 11 or later** (Recommended: Java 17+)
  - You can check your Java version with the following command:
    ```bash
    java -version
    ```
- **Maven** (For building the application)
  - Verify Maven is installed:
    ```bash
    mvn -v
    ```
- **Docker** (Optional, but recommended for easier execution)
  - Check Docker installation:
    ```bash
    docker --version
    ```
- **Git** (For cloning the repository)
  - Verify Git installation:
    ```bash
    git --version
    ```

## Clone the Repository

To get started, first clone the repository using Git:

```bash
git clone https://github.com/chidhvilaskacham/ultimate-cicd.git
```

Navigate into the spring-boot-app directory:

```bash
cd spring-boot-app
```
## Build the Application
The application is built using Maven. Follow the steps below to build the application and generate the artifacts:

**Step 1**: Clean and Package the Application

To clean any previously compiled files and package the application into a JAR file, run:

```bash
mvn clean package
```
The generated artifact (JAR file) will be located in the target/ directory.

The build output will also display any test results, errors, or warnings.

Run the Application Locally
You can run the Spring Boot application either using Java or Docker. Below are the steps for each method:

Using Java (Requires Java 11+)
Execute the following command to run the application:

```bash
java -jar target/spring-boot-web.jar
```
Access the application:

Open your browser and visit the following URL:
http://localhost:8080
This will load the application running locally on port 8080.

## Run the Application with Docker
Running the application with Docker is the preferred method as it isolates the environment and ensures consistency across different systems.

**Step 1**: Build the Docker Image
To build the Docker image for the application, use the following command:

```bash
docker build -t ultimate-cicd-pipeline:v1 .
```
-t assigns a tag (ultimate-cicd-pipeline:v1) to the image.
The . specifies the current directory as the build context.
This command will generate a Docker image that contains the Spring Boot application.

**Step 2**: Run the Docker Container
Now, run the Docker container with the following command:

```bash
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```
-d runs the container in detached mode (in the background).
-p 8010:8080 maps port 8080 of the container to port 8010 on your host machine.

**Step 3**: Access the Application
Once the container is running, open your browser and navigate to:

http://localhost:8010
You should see the application running, just like in the local Java execution.

Hurray!! 🎉
Congratulations, you've successfully set up and run the Spring Boot application using both Java and Docker. You can now use this setup to further explore and enhance your application or integrate it into your CI/CD pipeline.
