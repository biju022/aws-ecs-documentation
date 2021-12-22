# CREATING A ECS SERVICE

ECS Service: ECS Service is a managed AWS Service which can take care of Containers, health checks to make sure all services, tasks are running. If not, ECS Service auto deploys new tasks to give fault tolerance as well as High Availability

NOTE: Only proceed with ECS Service once you are sure that the TD which you created is working without any issues. It is recommended as ECS Service when created is attached to Load Balancer, Target groups and there can be issues happening from them also. To be sure, its not TD issue, fix all the TD issues and then move to ECS Service

STEP 1 - Choose the cluster where you want to create a service and Click on Create Service

STEP 2 - Here you need to choose
        
        Launch Type - Should be same as TD
        Task definition - Select your TD here
        Revision - Select the revision of your TD
        Cluster - Choose the cluster where you want to run this service
        Service name - Give a beautiful name to your Service
        Service Type - Leave default (Replica)
        Number of tasks - specify the no. of tasks you want to run (If you are testing, choose 1)
        Leave all the things to default and click Next at bottom of page

STEP 3 - In the next page,
        
        In Load Balancing section, 
        Choose Application LB
        Service IAM Role - ecsServiceRole
        Load Balancer - Choose your LB

        Container name:port - Choose your container and port and click add to LB
        Once added, in the below details
                                        Production listener port - Choose 80:HTTP (NOTE- SSL for all services is provided by LB, if not, then only choose 443)
                                        Target Group name - Choose Create new and s[ecify its name in right box
                                        Target Group Protocol - Choose HTTP/HTTPS according to TD (Usually its HTTP)
                                        Path pattern - Leave default
                                        Evaluation number - Add any number here, but it should not be present in the below showed table
                                        Health Check path - Choose the path where LB needs to check if task is giving proper response or not (Usually it is "/" Choose something else if you know what you are doing)
                                        That's it, Leave the rest to default and choose Next

STEP 4 - In the next page, Choose if you want to use AutoScale your tasks based on Memory/CPU/LB RequestCount. These settings vary according to needs (Default - Do not adjust the service’s desired count) 

STEP 5 - Click next and review the configuration. Once checked, you can create service. 

# CHECKS TO BE PERFORMED AFTER DEPLOYING SERVICE

STEP 1 - Check if there was any error while deploying ECS Service

STEP 2 - Check if the Desired task and Running tasks match in Service panel. If not, your task has an error or is still in deployment

STEP 3 - Once task is running, go to EC2 >> Target Groups >> Your_TG >> Check if the Health check is showing healthy. If its in initial state, wait a few mins and check. (If it goes unhealthy/Draining, you need to debug the issue. Refer Target Group's Docs for more info)

STEP 4 - Only if it goes healthy, then you can proceed to add Load Balancer rules as per requirement