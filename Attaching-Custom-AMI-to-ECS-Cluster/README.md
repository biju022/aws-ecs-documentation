# Attaching a Custom AMI for ECS Cluster

By default AWS recommends, using their own ECS optimised Amazon Linux AMI for ECS Clusters. But if you want to use a custom AMI, like Ubuntu/CentOS or anything for EFS or other purposes, you can use the below guide to attach an EC2 Instance launched via AutoScaling to an ECS Cluster

NOTE - Before you begin, make sure you have stopped Nginx/Apache or any services used in the EC2 and update all the packages. Check if any ports are used by any services using below command. This makes sure that when this EC2 is attached to ECS Cluster, you can be sure which ports are available for you. 
```
netstat -ntpl
```
The above command shows the port being used, as well as service which is using it

## Pre-Requisites 

Install docker, docker-compose as per official documentation. Below are the reference links

Docker - https://docs.docker.com/engine/install/

Docker-compose - https://docs.docker.com/compose/install/

## Installing ECS Container agent

ECS Container agent is the one responsible for attaching a particular EC2 instance to ECS Cluster. Once this container agent is running, there are two ways, EC2 gets attached

Check Docker version
```
sudo docker version
```
Enable ipv4 packet forwarding in Linux
```
sudo sh -c "echo 'net.ipv4.conf.all.route_localnet = 1' >> /etc/sysctl.conf"
sudo sysctl -p /etc/sysctl.conf
```
Create folders for ECS Config
```
sudo mkdir -p /etc/ecs && sudo touch /etc/ecs/ecs.config
```
Check Se-Linux Status and disable it
```
sestatus
sudo apt install policycoreutils
sestatus
```
NOTE - Before you run the ECS Agent - Change "ECS_CLUSTER" to your cluster name in "/etc/ecs/ecs.config"
Running ECS Agent
```
curl -o ecs-agent.tar https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-latest.tar
sudo docker load --input ./ecs-agent.tar
sudo docker run --name ecs-agent --detach=true --restart=always --volume=/var/run/docker.sock:/var/run/docker.sock --volume=/var/log/ecs:/log --volume=/var/lib/ecs/data:/data --net=host --env-file=/etc/ecs/ecs.config --env=ECS_LOGFILE=/log/ecs-agent.log --env=ECS_DATADIR=/data/ --env=ECS_ENABLE_TASK_IAM_ROLE=true --env=ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true amazon/amazon-ecs-agent:latest
docker ps
```

## POST INSTALLTION

STEP 1 - Once ECS Agent is  running, now you need to take an AMI of this Instance and create a launch template to use with AutoScaling. 

STEP 2 - Now create an AutoScaling group using the above Launch Template. 

STEP 3 - Once created, add the AutoScaling group in ECS Cluster's Capacity Provider section, so all instances are added to Cluster.

NOTE - If you forget create AutoScaling group, ECS Agent will not be able to communicate with ECS Cluster
