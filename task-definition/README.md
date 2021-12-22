# TASK DEFINITION CREATION GUIDE

> Task Definition (TD): A task definition is similar to docker-compose file or like a file which has configuration of the docker container which needs to be run. You can create a new TD using either GUI in AWS Console or JSON file

##USING AWS CONSOLE

When you create a new Task definition,

STEP 1 -  Specify which underlying infrastructure you need to use to run this task (AWS Fargate/ EC2/ External EX2 instances)

STEP 2 - Enter the TD name, Select

            Task role > ecsTaskExecutionRole 
            Network Mode > Bridge
            Task Execution IAM Role > None
            Task memory > [As per requirement]
            Task CPU > [As per requirement]
            For now, skip directly to Volumes panel at bottom of page (Will configure Containers in next step)
            Choose Add Volume > Enter the mount's name and the host path which you want to mount and save it. (NOTE: You can add any number of mounts here)

STEP 3 - Now scroll back to Container definitions panel and click Add Container

STEP 4 - Here,

            Container name - Enter the container's name here
            Image - Enter the image name (if public image) or Image URL from ECR
            In Port mappings - Enter the host ports to left and container port in right box
            Next, If you want to specify Environmental variables here, you can add as Key-Value pair in Environment panel at the bottom
            In Network settings, you need to specify the name of another container in the Links box, if the containers depend on each other (Ex:  Nginx depends on PHP container to parse, compile PHP code, so you need to add name of php container in nginx > Links box)
            In storage and logging, Mount points > Choose Source volume and specify the container path which needs to be mounted here. If you want it to be read-only, you can tick the below box
            In Log configuration, you need to tick Auto Configure CloudWatch Logs, so all logs can be accessed via CloudWatch
            Save the container at bottom

Create any new containers using same steps as in STEP 4

STEP 5 - Once done, verify all the configs once, (Mainly CloudWatch Logs, since they are most important to debug errors) and save the task

##USING JSON 

When you create a new Task definition,

STEP 1 -  Specify which underlying infrastructure you need to use to run this task (AWS Fargate/ EC2/ External EX2 instances)

STEP 2 - Scroll all the way down, and click "Configure via JSON"

STEP 3 - Paste the JSON data and save it. (Keep in mind, Copying an existing TD's JSON will have some extra parameters in it like TD's name, etc. JSON Editor will throw an error as below and you should only have the below configs in JSON)

Should only contain "family", "containerDefinitions", "volumes", "taskRoleArn", "networkMode", "requiresCompatibilities", "cpu", "memory", "inferenceAccelerators", "executionRoleArn", "pidMode", "ipcMode", "proxyConfiguration", "tags", "runtimePlatform", "placementConstraints"

STEP 4 - Go back to each container and enable CloudWatch Logs

STEP 5 - Verify all the configuration has been done in UI and save the task

TA DAA! Your Task Definition is ready to be deployed

# RUNNING A TASK 

To run as task, you need to open the TD in AWS Console and choose the TD revision which you want to run. Choose the "Run Task" option and a new window appears

Firstly, choose the Cluster you want to deploy this task, then choose EC2/Fargate/External as per your task and Click Run Task at bottom of page. 

Your task will be running now and will be shown in the respective Cluster. 

# COMMON ISSUES FACED WHEN RUNNING A TASK

1. RESOURCE : CPU/MEMORY Error

    This issue usually happens when there is not enough CPU/Memory available in any EC2 instances attached to Cluster
    FIX - 1. You can check Cluster > ECS Instances > and verify memory/CPU available for tasks and then reduce to available size in task definition
          2. You can use AutoScaling groups to increase number of EC2 intances attached to Cluster to get more Memory/CPU

2. ATTRIBUTE Error 

    This usually happens when the TD is created for one Cluster and you try to run in another cluster. Also happens when the AMI changes for the cluster.
    FIX - Create a new TD from scratch. Creating a new revision doesn't work. 

# COMMON TROUBLESHOOTING METHOD

STEP 1 - Open the Task ID which you are facing issue with, check the EC2 instance ID for the task and login via SSH to that EC2 instance

STEP 2 - Run the below command to get the list of running as well as stopped containers
```
docker ps -a
```
STEP 3 - Check the task which you are facing issue and copy the CONTAINER ID of it

STEP 4 - Run the below command to inspect the container and get the detailed output of error
```
docker inspect CONTAINER_ID
```

# CHECKING VOLUME MOUNTS FOR A TASK/SERVICE

STEP 1 - Once the task/service is running, check the EC2 instance ID where it is running and login to it via SSH 

STEP 2 - STEP 2 - Run the below command to get the list of running containers
```
docker ps
```
STEP 3 - Check the task which you want to check volume mount and copy the CONTAINER ID of it

STEP 4 - Run the below command to login into container
```
docker exec -it CONTAINER_ID /bin/bash
```
If above command doesn't work, try this one
```
docker exec -it CONTAINER_ID /bin/sh
```
STEP 5 - Once inside container, check the container path you have mounted and verify files exist, which should be same as host path