# Project Diagrams and Screenshots

Visual documentation of the AWS scalable web application infrastructure.

---

## 1. Architecture Diagram

![Architecture](../aws-images/Diagram.png)

---

## 2. VPC Setup

![VPC](../aws-images/VPC.png)

---

## 3. Subnets Configuration

- Two public subnets (EC2)
- Two private subnets (RDS)

![Subnets](../aws-images/Subnets.png)

---

## 4. Security Groups

- EC2 SG for HTTP/HTTPS/SSH
- ALB SG for public access

![Security Groups](../aws-images/SecurityGroup.png)

---

## 5. Launch Template or EC2 Configuration

- AMI: Amazon Linux 2
- Instance type: `t2.micro`
- User data to install web server

![Launch Template](../aws-images/LuanchTemplate.png)

---

## 6. EC2 Instances

- Managed by Auto Scaling Group

![Instances](../aws-images/Instances.png)

---

## 7. Application Load Balancer (ALB)

- Internet-facing
- Listener on port 80
- Target group: EC2

![ALB](../aws-images/LoadBalancer.png)

---

## 8. Auto Scaling Group (ASG)

- Min: 2, Desired: 2, Max: 5
- CPU-based scaling policy

![ASG](../aws-images/AutoScalingGroups.png)

---

## 9. Amazon RDS (Multi-AZ)

- MySQL/PostgreSQL engine
- Private subnet
- No public access

![RDS](../aws-images/RDS-DB.png)

---

## 10. CloudWatch and SNS Monitoring

- Alarms based on CPU usage
- SNS Topic created and email subscribed

![CloudWatch](../aws-images/Cloudwatch.png)

---

## 11. Alarm Actions

- Scale out: CPU > 80%
- Scale in: CPU < 30%
- Email notifications via SNS

![Alarms](../aws-images/SNS.png)

---

## 12. Web Page - Testing the Application

Accessed via the ALB DNS name

![Web Page](../aws-images/WebPage.png)

---
