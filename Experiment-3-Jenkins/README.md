# Experiment 3: Jenkins and GitHub Integration

## Aim
To integrate a GitHub repository with Jenkins and automate project execution.

## Build methods
1. Manual build
2. Periodic build using Poll SCM
3. Event-based build using GitHub Webhooks

## Application
A simple HTML page used to verify repository checkout.

## Jenkins Install
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21  

## Result
The application repository is prepared for Jenkins integration.