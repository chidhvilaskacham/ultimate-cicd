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
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] | https://pkg.jenkins.io/debian-stable binary/ | sudo tee  /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt install jenkins=2.426.3 -y
    sudo systemctl enable jenkins
    sudo systemctl start jenkins
    ```

2.  **Access Jenkins:** `http://<instance-public-ip-address>:8080`.

3.  **Retrieve Initial Admin Password:**

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

4.  **Configure Jenkins Pipeline:**
    * Create a new pipeline job from the Jenkins UI.
    * Configure to use `Jenkinsfile` from the provided repository: `https://github.com/chidhvilasKacham/ultimate-cicd.git`.
    * Update `spring-boot-app/JenkinsFile` with your SonarQube URL, DockerHub ID and `spring-boot-app-manifests/deployment.yml` with your DockerHub ID and GitHub ID.

5.  **Install Jenkins Plugins:**
    * Navigate to Jenkins > Manage Jenkins > Plugins > Available plugins.
    * Install: `Docker Pipeline`, `SonarQube Scanner`.

6.  **SonarQube Setup:**

    ```bash
    sudo su -
    sudo adduser sonarqube
    sudo su - sonarqube
    wget [https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip](https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip)
    sudo apt install unzip
    unzip sonarqube-9.4.0.54424.zip
    chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
    chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
    cd /home/sonarqube/sonarqube-9.4.0.54424/bin/linux-x86-64/
    ./sonar.sh start
    ```

    * Access SonarQube at `http://<instance-public-ip-address>:9000`.
    * Login with `admin` / `admin` and change the password.

7.  **Configure Jenkins Credentials:**
    * Navigate to Jenkins > Manage Jenkins > Credentials > System > Global credentials > Add Credentials.
    * **SonarQube Token:**
        * Kind: `Secret text`.
        * ID: `sonarqube`.
        * Secret: SonarQube Token.
    * **Docker Hub Credentials:**
        * Kind: `Username with password`.
        * ID: `docker-cred`.
        * Username: Your Docker Hub username.
        * Password: Your Docker Hub password.
    * **GitHub Personal Access Token:**
        * Kind: `Secret text`.
        * ID: `github`.
        * Secret: GitHub Personal Access Token.

8.  **Install Docker:**

    ```bash
    sudo apt update
    sudo apt install docker.io
    sudo usermod -aG docker $USER
    sudo usermod -aG docker jenkins
    sudo systemctl restart docker
    ```

9.  **Run the Jenkins Pipeline:** Trigger the pipeline from the Jenkins UI.

### 2. Argo CD Setup (CD)

1.  **Minikube/Kubernetes Setup:**

    ```bash
    New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
    Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
    $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
    if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
      [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
    }
    minikube start --driver=docker
    ```



2.  **Install Argo CD Operator:**

    ```bash
    curl -sL [https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.24.0/install.sh](https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.24.0/install.sh) | bash -s v0.24.0
    kubectl create -f [https://operatorhub.io/install/argocd-operator.yaml](https://operatorhub.io/install/argocd-operator.yaml)
    kubectl get csv -n operators
    kubectl get pods -n operators
    ```

3.  **Deploy Argo CD:**

    * Create `argocd-basic.yml` using vim or nano

        ```yaml
        apiVersion: argoproj.io/v1alpha1
        kind: ArgoCD
        metadata:
          name: example-argocd
          labels:
            example: basic
        spec: {}
        ```

    * Apply the configuration: `kubectl apply -f argocd-basic.yml`.
    * Edit Argo CD service: `kubectl edit svc example-argocd-server` and change `type: ClusterIP` to `type: NodePort`.
    * Or You need to edit the ArgoCD custom resource example-argocd and look for `spec.server.service.type` to `NodePort` .

5.  **Retrieve Argo CD Password:**

    ```bash
    kubectl get secret
    kubectl edit secret example-argocd-cluster
    echo -n <admin.password> | base64 -d
    ```

6.  **Configure Argo CD:**
    * Access Argo CD UI using the `minikube service example-argocd-server` URL.
    * Login with `admin` and the decoded password.
    * Add the GitHub repository: `https://github.com/chidhvilaskacham/ultimat-cicd.git`.
    * Create an Argo CD application to deploy the `spring-boot-app` using the `spring-boot-app-manifests` directory.

