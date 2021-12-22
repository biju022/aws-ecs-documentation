# LOAD BALANCER

> Load Balancer - AWS Managed service, which works as per its name - Load Balances traffic with attached Target Groups. 

You can load balance between multiple Target groups with specification of percentage of Balancing also. 

## ADDING ACM CERTIFICATE TO AWS ELB (Elastic Load Balancer)

ACM certificate is provided free by AWS when you use LB. Search ACM in AWS Console and request a new certificate for domains which are required. Once requested, you need to validate that you own the domain by suggested method in that panel. Once validated, you will see SSL/TLS certificate in LB Panel.

Once it is visible in LB Panel, use "Add" option to enable the certificate

## USAGE OF LISTENER, RULES

Listener is the port in which LB listens and then it uses Rules added in Listener to redirect traffic to desired destination

NOTE- By default, LB allows 100 rules per listener. So, while adding, if multiple domains point to same Target group, add them together in single Rule. A single rule can have a maximum of 5 domains in it. So Technically, You can map a maximum of 500 domains in one single listener