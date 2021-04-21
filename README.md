# SCA FINAL PROJECT

## Project: Deploying A Noteapp With A MongoDB To Kubernetes Using Jenkins CI/CD Pipeline

I’ll be using Terraform to Provision the Kubernetes Cluster and The Jenkins Server.

Link to Terraform Repository: `https://github.com/ToluwalopeAyo/Terraform.git`

Link to App Repository: `https://github.com/ToluwalopeAyo/Kubernetes-Noteapp.git`

### Running The Application Locally

Create a config.env file in the root of the project and store a variable for the database to connect to the MongoDB database and the session-key

```
npm install
node app.js
```
The app is open on port 8000

### Containerising The Application

The Dockerfile is at the root of the project

```docker build -t noteapp .```

This would create a docker image of the application that we would run later

To run the containerised app with the mongodb database, the database and the application has to be in the same network so that they can communicate with each other. Create a new network

``` docker network create noteapp-net```

We can now run the mongodb and the app

```docker run --name=mongodb -d --network=noteapp-net mongo```

Mongodb is the name of the container while mongo is the name of the image

```
docker run --name=noteapp -dp 8000:8080 -e DATABASE=mongodb://mongo:27017/noteapp -e session_key=someRandomString --network=noteapp-net noteapp
```

The application would connect to the database and would be open on port 8080 of the localhost

### The Jenkins Server

- Install Jenkins and start it
- Install docker and add jenkins to the docker group 

```sudo usermod -a -G docker jenkins```

- Restart Jenkins

```sudo systemctl restart jenkins```

- Install Kubectl

- Login to Jenkins

- Install Docker Pipeline and Google Kubernetes Plugin

- Create a Service Account with **Kubernetes Engine Admin** permission and created a json key for iy

- Add dockerhub and service account credentials to give jenkins permission to Kubernetes and Docker hub

Create a github-webkook in the github repository with the ip address of the Jenkins Server `http://ip-address:8080/github-webhook/`

### Configuring The Pipeline
