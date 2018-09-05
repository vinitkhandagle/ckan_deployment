# ckan_stuff
## This repo contains the Ansible playbooks to deploy ckan

### Information:
The repository has two main files for creating the AWS instance and the role to setup the instance with Jenkins and the Jenkins Job to deploy ckan.Jenkins is deployed as a docker container on the AWS instance using the host's docker process. 
All the CKAN containers are also deployed on the same host.

I have made use of the docker compose for ckan in the Job file for deploying CKAN through Jenkins
