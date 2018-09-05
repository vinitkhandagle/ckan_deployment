# ckan_stuff
## This repo contains the Ansible playbooks to deploy ckan

### Information:
The repository has two main files for creating the AWS instance and the role to setup the instance with Jenkins and the Jenkins Job to deploy ckan.Jenkins is deployed as a docker container on the AWS instance using the host's docker process. 
All the CKAN containers are also deployed on the same host.

I have made use of the docker compose for ckan in the Job file for deploying CKAN through Jenkins.

### To Deploy using this repo

1. Clone the repo
```
git clone https://github.com/vinitkhandagle/ckan_deployment.git
cd ckan_deployment
```

2. Export your AWS access key and Secret Key
```
export AWS_ACCESS_KEY_ID='<your AWS_ACCESS_KEY>'
export AWS_SECRET_ACCESS_KEY='<your AWS_SECRET_KEY>'
```

3. Run the Server Installation ansible playbook
```
ansible-playbook aws_server.yml -vvvv
```

