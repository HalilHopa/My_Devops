# Project-204: Docker Swarm Deployment of Phonebook Application (Python Flask) with MySQL

## Description

This project aims to deploy the Phonebook Application web application with Docker Swarm on Elastic Compute Cloud (EC2) Instances by pulling the app images from the AWS Elastic Container Registry (ECR) repository.

## Problem Statement

![Project_204](project-204.png)

- The company has recently started a project that aims to serve as phonebook web application. My teammates have started to work on the project and developed the UI and backend part of the project and they need your help to deploy the app in development environment.

- I requested to deploy the Phonebook Application in the development environment on Docker Swarm on AWS EC2 Instances using Terraform to showcase the project. To do that I;

  - Created a new public repository for the project on GitHub.

  - Created docker image using the `Dockerfile` from the base image of `python:alpine`.

  - Deploy the app on swarm using `docker compose`. To do so on the `Compose` file;

    - Created a MySQL database service with one replica using the image of `mysql:5.7`;

      - attach a named volume to persist the data of database server.

      - attach `init.sql` file to initialize the database using `configs`.

    - Configured the app service to;

      - pull the image of the app from the AWS ECR repository.

      - deploy the one app for each swarm nodes using `global` mode.

      - run the app on `port 80`.

    - Used a user-defined overlay network for the services.

  - Push the necessary files for your project from local repo to the new github repo (phonebookswarm).

- I am also requested; to use AWS ECR as image repository, to create Docker Swarm with 3 manager and 2 worker node instances, to automate the process of Docker Swarm initialization through Terraform in the development environment. To achieve this goals, I configured Terraform configuration file using the followings;

  - The application should run on Amazon Linux 2 EC2 Instance

  - EC2 Instance type can be configured as `t2.micro`.

  - To use the AWS ECR as image repository;

    - Enable the swarm node instances with IAM Role allowing them to work with ECR repos using the instance profile.

    - Install AWS CLI `Version 2` on swarm node instances to use `aws ecr` commands.

  - To automate the process of Docker Swarm initialization;

    - Install the docker and docker-compose on all nodes (instances) using the `user-data` bash script.

    - Create the leader manager node of the swarm. Within the `user-data` script;

      - Set the first manager node hostname as `Leader-Manager`.

      - Initialize Docker swarm.

      - Create a docker service named `viz` on the manager node on port `8080` using the `dockersamples/visualizer` image, to monitor the swarm nodes easily.

      - Build the docker image from the GitHub URL of the new project repo and tag it appropriately to push it on ECR repo. (Note: Do not forget to install Git to enable Docker to work with git commands)

      - Download `docker-compose.yml` file from the repo and deploy application stack on Docker Swarm.

    - Create two manager nodes of the swarm. Within the `user-data` script;

      - Install the python `ec2instanceconnectcli` package for `mssh` command.

      - Connect from manager node to the `Leader-Manager` to get the `join-token` and join the swarm as manager node using `mssh` command.
    
    - Create two worker nodes of the swarm. Within the `user-data` script;

      - Install the python `ec2instanceconnectcli` package to use `mssh` command.

      - Connect from worker node to the `Leader-Manager` to get the `join-token` and join the swarm as worker node using `mssh` command.

  - Create a security group for all swarm nodes and open necessary ports for the app and swarm services.

  - Create an image repository on ECR for the app.

  - Tag the swarm node instances appropriately as `Docker-Swarm-Manager  <Number>/Docker-Swarm-Worker <Number>` to discern them from AWS Management Console.

  - The Web Application should be accessible via web browser from anywhere.

  - Phonebook App Website URL, Visualization App Website URL should be given as output by Cloudformation Service, after the stack created.

## Project Skeleton

```text
204-docker-swarm-deployment-of-phonebook-app-on-python-flask-mysql (folder)
|
|----readme.md            # (Definition of the project)
|----cfn-template.yml     # (Cloudformation template-Optional)
|----phonebook-app.py     # Given  (Python Flask Web Application)
|----requirements.txt     # Given  (List of Flask Modules/Packages)
|----init.sql             # Given  (SQL statements to initialize db)
|----main.tf              # To be delivered (Terraform configuration file)
|----Dockerfile           # To be delivered 
|----docker-compose.yml   # To be delivered 
|----templates
        |----index.html      # Given  (HTML template)
        |----add-update.html # Given  (HTML template)
        |----delete.html     # Given  (HTML template)
```

## Project Steps:

- Creating Docker File : For the create alpine image
- Create docker-compose.yaml: For the services we need (database and app server) 
- Push to my github Dockerfile and docker-compose


## Expected Outcome

![Phonebook App Search Page](./search-snapshot.png)

### At the end of the project,

- demonstrate how to configure Dockerfile and docker-compose files.

- set up a Docker Swarm cluster to work with AWS ECR using Terraform.

- deploy an application stack on Docker Swarm.

- create and configure AWS ECR from the AWS CLI.

- use Docker commands effectively to tag, push, and pull images to/from ECR.

- demonstrate bash scripting skills using `user data` section in Terraform to install and setup Docker and application environment on EC2 Instances.

- demonstrate their configuration skills of AWS EC2, IAM Policy, Role, Instance Profile, and Security Group.

- configure Terraform template to use AWS Resources.

- apply git commands (push, pull, commit, add etc.) and Github as Version Control System.

## Resources

- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)

- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/)

- [Docker Reference Page](https://docs.docker.com/reference/)

- [EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Connect-using-EC2-Instance-Connect.html)
