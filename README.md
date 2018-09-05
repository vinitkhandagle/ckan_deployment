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
This setups up the instance for deployment of Docker, Jenkins and all the necessary docker information.
This ansible run also creates an ssh key `ckankey` and places in your `$HOME/.ssh/` folder for later use.

4. If you check ur AWS account you can see the instance. to better manage the inventory note down the IP of the instance
    #### Pro tip
    create a ssh_config entry for the host. I have included a host file in the repo with a group aws1
 ```
Host aws1
    identityfile "/Users/vinitk/.ssh/ckankey.pem"
    hostname 35.154.175.62
    user ubuntu
    port 22
```   
I have a hosts file `hosts` for inventory in the repo with `[aws1]` group you can add the host there after configuring your ssh config file. It just makes lot of things easier. 

4. Once you have done with configuring the inventory file all you would need to do now is run the deploy play in the repo
```
ansible-playbook -T 300 -i hosts deploy_ckan.yml -vvv
```
The timeout interval is set a bit higher depending on the speed of the downloading on the host and creating the Jenkins container.

5. After the deploy script is done executing you can go to the public ip of the instance on 8080 to see the Jenkins container and the ckan job configured. `http://public_ip:8080`

This ansible run deploys the Jenkins container on the host with a Jenkins Job to deploy Ckan with docker compose.

6. The Jenkins job `ckan_install` can be run to deploy CKAN as docker containers on the host.
    This deploys containers for ckan application, a postgres container, redis container and a solr container.

```
ubuntu@ip-172-31-2-11:~$ sudo docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                               NAMES
356c15ec3fbb        docker_ckan                 "/ckan-entrypoint.sh…"   About an hour ago   Up About an hour    0.0.0.0:5000->5000/tcp              ckan
e4c04fb785ae        redis:latest                "docker-entrypoint.s…"   About an hour ago   Up About an hour    6379/tcp                            redis
27e3d59ae1f9        clementmouchet/datapusher   "python datapusher/m…"   About an hour ago   Up About an hour    0.0.0.0:8800->8800/tcp              datapusher
9ee71a5f8d6d        docker_solr                 "docker-entrypoint.s…"   About an hour ago   Up About an hour    8983/tcp                            solr
194c29fe6125        docker_db                   "docker-entrypoint.s…"   About an hour ago   Up About an hour    5432/tcp                            db
679ecb8c6c9b        dockerfile                  "/sbin/tini -- /usr/…"   About an hour ago   Up About an hour    0.0.0.0:8080->8080/tcp, 50000/tcp   MYJENKINS
```

7. After the containers are deployed you should be able to go to `http://public_ip:5000` to see the ckan application deployed on the container
