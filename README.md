# DevOps-Project
Unified Automation Stack Implementation
# Capstone Project Readme

## Overview

This capstone project aims to automate the deployment of a web application using Jenkins Freestyle jobs, Docker, and Ansible. The web application's source code is hosted on GitHub, and the deployment process involves building a Docker image and deploying containers using Ansible.

## Problem Statement

Automate the deployment of a web application using Jenkins Freestyle jobs, Docker, and Ansible. The web application's source code is hosted on GitHub, and the deployment should involve building a Docker image and deploying containers using Ansible.

## Objectives

1. Create a Jenkins Freestyle job to automate the deployment process.
2. Integrate Jenkins with a Git repository on GitHub to fetch the latest source code.
3. Build a Docker image for the web application.
4. Deploy the application to target servers using Ansible and Docker containers.

## Tasks

### 1. Jenkins Configuration

- Install and configure Jenkins on the desired server.
- Ensure Jenkins can access the internet to download plugins.
- Set up authentication and security settings as per your organization's requirements.

### 2. GitHub Integration

- Create a GitHub repository to store the web application's source code.
- Configure Jenkins to integrate with the GitHub repository using webhook triggers.
- Create a personal access token in GitHub with the required permissions for Jenkins to access the repository.

### 3. Jenkins Freestyle Job

- Create a new Freestyle job in Jenkins.
- Configure the job to fetch the latest source code from the GitHub repository using Git.
- Add an "Execute shell" build step to build a Docker image for the web application (e.g., `docker build -t my-web-app:latest .`).
- In the same "Execute shell" build step, use the `ansible-playbook` command to execute the deployment playbook, which includes deploying Docker containers.

### 4. Docker Configuration

- Create a `Dockerfile` for your web application.
- Ensure that the `Dockerfile` copies your web application code into the image and sets up the necessary runtime environment.

### 5. Ansible Configuration

- Create Ansible playbooks and roles for deploying Docker containers of the web application.
- Set up an Ansible inventory file with details of target servers.
- Ensure Ansible can connect to target servers via SSH or any other required method.

### 6. Testing and Validation

- Test the Jenkins Freestyle job by manually triggering a build.
- Ensure that the entire deployment process, including Docker image building and container deployment, works as expected.
- Monitor the Jenkins job console output for any errors or issues.

### 7. Automation and Continuous Integration (Optional)

- Set up webhooks or periodic polling in Jenkins to automatically trigger the Freestyle job builds when changes are pushed to the GitHub repository.
- Configure post-build actions, such as email notifications or Slack notifications, to notify the team of build results.

### 8. Documentation and Maintenance

- Document the Freestyle job configuration, including any environment-specific settings or secrets.
- Document the Dockerfile and how the Docker image is built.
- Ensure that Jenkins, Ansible, and Docker configurations are well-documented for maintenance and future reference.

## Server Configuration

### Master Instance

- Location: `ip-172-31-46-177`
- Directory Contents:
  - `Dockerfile`
  - `a.sh`
  - `provision.yaml`

## Dockerfile

```dockerfile
FROM ubuntu
RUN apt update
RUN apt install apache2 -y
RUN echo "hello world" > /var/www/html/index.html
EXPOSE 88
ENTRYPOINT apachectl -D FOREGROUND
```

## Ansible Provisioning YAML (`provision.yaml`)

```yaml
- name: Pull the Docker image
  hosts: web-servers
  become: yes
  tasks:
    - name: Pull the Docker image
      docker_image:
        name: docker.io/ubuntu123am/my-apache-image
        source: pull
        state: present
```

## Kubernetes Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-apache-deployment
spec:
  replicas: 3  # Number of pod replicas
  selector:
    matchLabels:
      app: my-apache-pod
  template:
    metadata:
      labels:
        app: my-apache-pod
    spec:
      containers:
      - name: my-apache-container
        image: docker.io/ubuntu123am/my-apache-image
        ports:
        - containerPort: 80
