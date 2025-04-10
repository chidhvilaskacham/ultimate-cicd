![image](https://github.com/user-attachments/assets/f9f58250-ec7a-47d9-8688-392bf6179f31)

# Automated CI/CD Pipeline for Java Applications with Jenkins, SonarQube, Docker, and Argo CD

This repository provides a comprehensive guide and configuration for setting up an automated Continuous Integration/Continuous Delivery (CI/CD) pipeline for Java applications. The pipeline utilizes Jenkins for CI, SonarQube for static code analysis, Docker for containerization, and Argo CD for GitOps-based CD on Kubernetes.

## Overview

The pipeline automates the following processes:

1.  **Continuous Integration (CI):**
    * Source code retrieval from GitHub.
    * Build process using Maven.
    * Static code analysis with SonarQube.
    * Docker image creation and push to Docker Hub.
2.  **Continuous Delivery (CD):**
    * Automated deployment to Kubernetes using Argo CD.
    * GitOps approach for declarative application deployment.

## Prerequisites

* AWS EC2 Instance (for Jenkins and SonarQube)
* Java Development Kit (JDK) 11+
* Maven
* Docker
* Git
* GitHub Account
* Docker Hub Account
* Minikube or Kubernetes Cluster
* kubectl

## Installation and Setup

### 1. Jenkins Setup (CI)

1.  **Install Jenkins:**
    ```bash
    sudo apt update
    sudo apt install openjdk-11-jre
    # ... (Jenkins installation commands from provided script) ...
    ```
2.  **Access Jenkins:** `http://<ec2-instance-public-ip-address>:8080`.
3.  **Retrieve Initial Admin Password:**
    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
4.  **Configure Jenkins Pipeline:**
    * Create a new pipeline job.
    * Configure to use `Jenkinsfile` from the provided repository.
    * Update `Jenkinsfile` and `deployment.yml` with relevant credentials and URLs.
5.  **Install Jenkins Plugins:**
    * Docker Pipeline
    * SonarQube Scanner
6.  **SonarQube Setup:**
    ```bash
    sudo adduser sonarqube
    sudo su - sonarqube
    # ... (SonarQube installation commands from provided script) ...
    ```
7.  **Configure Jenkins Credentials:**
    * SonarQube Token (ID: `sonarqube`)
    * Docker Hub Credentials (ID: `docker-cred`)
    * GitHub Personal Access Token (ID: `github`)
8.  **Install Docker:**
    ```bash
    sudo apt update
    sudo apt install docker.io
    # ... (Docker installation commands from provided script) ...
    ```
9.  **Run the Jenkins Pipeline:** Trigger the pipeline to execute the CI process.

### 2. Argo CD Setup (CD)

1.  **Minikube/Kubernetes Setup:**
    ```bash
    # (Minikube setup commands from provided script)
    ```
2.  **Install kubectl:**
    ```bash
    # (kubectl installation commands from provided script)
    ```
3.  **Install Argo CD Operator:**
    ```bash
    # (Argo CD Operator installation commands from provided script)
    ```
4.  **Deploy Argo CD:**
    * Create `argocd-basic.yml` and apply it.
    * Expose Argo CD service using `NodePort`.
5.  **Retrieve Argo CD Password:**
    ```bash
    # (Commands to get Argo CD admin password from provided script)
    ```
6.  **Configure Argo CD:**
    * Access Argo CD UI.
    * Add the GitHub repository containing the application manifests.
    * Create an Argo CD application to deploy the Java application.

## Pipeline Workflow

1.  Developers commit code to the GitHub repository.
2.  Jenkins triggers the pipeline.
3.  Jenkins builds the application, runs SonarQube analysis, and creates a Docker image.
4.  The Docker image is pushed to Docker Hub.
5.  Argo CD detects changes in the Git repository and updates the Kubernetes deployment.
6.  Kubernetes deploys the new application version.
