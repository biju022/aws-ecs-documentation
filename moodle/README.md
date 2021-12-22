# MOODLE

To run Moodle via Load Balancer, add the below lines to config.php in moodle folder

```
$http_user_agent = $_SERVER['HTTP_USER_AGENT'];
if (strstr($http_user_agent, "ELB-HealthChecker")) {
    echo "ok";
    die;
}
```
Also make sslproxy to true
```
$CFG->sslproxy = true;
```

Reference Link - https://sourcetut.com/how-to-operate-moodle-with-aws-load-balancer-https-from-internet-but-http-behind-4fccb/