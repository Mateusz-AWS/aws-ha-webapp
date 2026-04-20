# Highly Available Web Application on AWS

This project simulates a production-like highly available web application environment on AWS.

Live Demo: http://alb-web-996721267.us-east-1.elb.amazonaws.com/

This setup ensures high availability by distributing traffic across multiple instances and automatically replacing unhealthy ones.

---

## Architecture

![Architecture](Architecture.png)

The application is deployed across multiple Availability Zones using an Application Load Balancer and Auto Scaling Group to ensure high availability and fault tolerance.

---

## Technologies Used

* Amazon EC2  
* Application Load Balancer (ALB)  
* Auto Scaling Group (ASG)  
* Amazon CloudWatch  
* Amazon SNS  
* Amazon VPC  
* Launch Template  
* HTML (simple web application)  

---

## Application

The application is a simple web page deployed on EC2 instances.

It displays:

* Server name (web-1 / web-2)  
* Basic project information  

The load balancer distributes traffic between instances.

---

## EC2 Instances

EC2 instances host the web application and are managed by Auto Scaling.

![EC2 Instances](screenshots/instances.jpg)

---

## Load Balancer

Application Load Balancer distributes incoming traffic across multiple EC2 instances.

![Load Balancer](screenshots/load-balancers.jpg)

---

## Load Balancing

Traffic is routed between different instances.

![Load Balancing 1](screenshots/load-balancing-1.jpg)

![Load Balancing 2](screenshots/load-balancing-2.jpg)

---

## Target Group Health Checks

Target groups ensure only healthy instances receive traffic.

![Target Group Health](screenshots/target-groups-health.jpg)

---

## Auto Scaling

Auto Scaling automatically replaces unhealthy instances and maintains the desired number of instances across multiple Availability Zones.

![Auto Scaling Activities](screenshots/auto-scaling-activities.jpg)

---

## CloudWatch Alarm

CloudWatch monitors CPU usage and triggers an alarm when the threshold is exceeded.

![Alarm Graph](screenshots/alarm-graph.jpg)

---

## SNS Notification

When the alarm is triggered, an email notification is sent using SNS.

![SNS Notification](screenshots/sns-email-notification.jpg)

---

## Features

* High availability across multiple Availability Zones  
* Automatic scaling and self-healing infrastructure  
* Load balancing across instances  
* Health checks and failover  
* Monitoring and alerting  
* Fault tolerance  

---

## How to deploy

1. Create a Launch Template with User Data to install and configure the web server automatically  
2. Launch EC2 instances (Amazon Linux) using the Launch Template  
3. Deploy a simple HTML application  
4. Create a Target Group  
5. Register EC2 instances in the Target Group  
6. Create an Application Load Balancer  
7. Configure listener (HTTP :80 → Target Group)  
8. Create an Auto Scaling Group (min: 2, desired: 2) across multiple Availability Zones  
9. Attach the Target Group to the Auto Scaling Group  
10. Configure a CloudWatch alarm (CPU > 50%)  
11. Create an SNS topic and email subscription  

The application is accessible via the Load Balancer DNS.

---

## Security Considerations

- Security Groups restrict access (ALB → EC2 only on HTTP/HTTPS)  
- EC2 instances should use IAM roles with least privilege  
- In production, HTTPS (ACM) and AWS WAF should be implemented  

---

## Lessons Learned

- Health checks are critical – if they fail, instances are removed from the load balancer  
- Misconfigured security groups can block traffic between ALB and EC2  
- Instances may not register in Target Group if ports or health check paths are incorrect  
- Monitoring and alerting are essential for production environments  

---

## Potential Issues

- EC2 instance not appearing in Target Group  
  - Check security groups and ports  
  - Verify health check path  

- High CPU usage triggering alarms  
  - Can simulate load for testing  

- Load balancer not responding  
  - Check listener configuration  
  - Verify target group health status  

---

## Future Improvements

- Infrastructure as Code (Terraform)  
- HTTPS (SSL certificate via ACM)  
- CI/CD pipeline (GitHub Actions)  
- Logging with CloudWatch Logs  

---

## Author

Mateusz
