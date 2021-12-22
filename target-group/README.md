# TARGET GROUP

> Target Group - Target group is a group of ports/paths which have are monitored by AWS to ensure all services/tasks/underlying websites, services are functioning and give proper response as intended. Most important part, for Load Balancing.

Creation of Target Group (TG) will be already done by ECS Service

# TROUBLESHOOTING TG ERRORS

Firstly, you need to check the health check path for TG and for verifying open the Task's IP and go to the path mentioned in health check 
Example - If you have specified "/" as path and your task's IP is 1.1.1.1 then open "1.1.1.1" in browser. It should open something. Or best recommended way is to use 

```
curl -v 1.1.1.1
```
Above command which gives verbose output and you can see the HTTP response code. This response code should match the one in TG. Else TG will become unhealthy/Draining

If your path is /blah then you should use below command to check output
```
curl -v 1.1.1.1/blah
```

##Target Group stages

Initial - The target group is registering the target and is in initialization process

Healthy - Target has been registered and Load Balancer can successfully transfer traffic from LB >> TG >> ECS Service

Unhealthy/Draining - Some issue with response received by target group by the target i.e your task