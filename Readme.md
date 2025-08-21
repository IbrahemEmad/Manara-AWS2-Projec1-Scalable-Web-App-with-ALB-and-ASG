# Auto-Scaling Web Application on AWS

## Overview
A secure, highly available, and auto-scaling web application using:
- EC2 instances
- Application Load Balancer (ALB)
- Auto Scaling Groups (ASG)
- Amazon RDS
- CloudWatch and SNS for monitoring

## Features

- Load Balanced traffic using ALB
- Auto Scaling based on CPU utilization
- Web app hosted on EC2 instances across multiple Availability Zones
- Real-time Monitoring and Notifications using CloudWatch + SNS
- RDS Multi-AZ database backend
- IAM role-based permissions for secure access

---

## Tech Stack

- **Amazon EC2**
- **Application Load Balancer (ALB)**
- **Auto Scaling Group (ASG)**
- **CloudWatch + SNS**
- **IAM Roles**
- **Amazon RDS**

---

## Architecture Diagram

![Architecture](docs/Diagram.png)

---

##  Step-by-Step Deployment Guide

### 1. **VPC Setup**
- Use **default VPC** or create a new one 
- **VPC Name**: ScalableAppVPC-vpc

![VPC](../aws-images/VPC.png)

- Public Subnets (2) for EC2 + Private Subnets (2) for RDS.
- Attach Internet Gateway and configure route tables.

![Subnets](../aws-images/Subnets.png)



### 2. **Security Groups**
- **Security Groups Name**: (ALP-SecurityGroups)
- EC2 SG: Allow HTTP (80), HTTPS (443), SSH (22 from your IP)
- ALB SG: Allow HTTP/HTTPS from anywhere (0.0.0.0/0)
- RDS SG: Allow DB ports only from EC2 SG

![Security Groups](../aws-images/SecurityGroup.png)

### 3. **IAM Role for EC2**
Attach a role with:
- `AmazonEC2ReadOnlyAccess`
- `CloudWatchAgentServerPolicy`
- `AmazonSSMManagedInstanceCore`
- `AmazonS3Rlole`

### 4. **Launch Template**
- **Launch Template Name**: (web-app-template)
- AMI: Amazon Linux 2 or Ubuntu
- Instance Type: `t2.micro`
- Attach IAM Role

![Launch Template](../aws-images/LuanchTemplate.png)

`user-data`:
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
cat <<'EOF' > /var/www/html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Welcome</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f6f8;
      color: #333;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    h1 {
      font-size: 2rem;
      background: #fff;
      padding: 20px 30px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>💫Welcome to Your Scalable Web App💫</h1>
    <br></br>
    <p>A secure, highly available, and auto-scaling web application using: EC2 instances, Application Load Balancer (ALB), Auto Scaling Groups (ASG), Amazon RDS, CloudWatch and SNS for monitoring</p>
    <br></br>
    <footer>© 2025 Ibrahem Emad. All rights reserved.</footer>
  </div>
</body>
</html>
EOF
```

### 5. Create Application Load Balancer (ALB)
- **ALB Name**: (web-ALB)
- Listener on port 80
- Target group: EC2
- subnets: public subnet-1, public subnet-2 
- security-groups: ALP-SecurityGroups

![ALB](../aws-images/LoadBalancer.png)

### 6. Create Auto Scaling Group (ASG)
- **ASG Name**: (web-ASG)
- auto-scaling-group-name web-asg 
- launch-template "LaunchTemplateName = web-app-template
- min-size 2 , max-size 5 , desired-capacity 2 
- vpc-zone-identifier "subnet-1,subnet-2" 
- Scale-out policy (CPU >80%)

![ASG](../aws-images/AutoScalingGroups.png)

### 7. CloudWatch Alarm
- aws cloudwatch put-metric-alarm 
- alarm name:  SNS Alarm 
- metric-name CPUUtilization 
- threshold 80 

![CloudWatch](../aws-images/Cloudwatch.png)
![Alarms](../aws-images/SNS.png)

### 8. RDS - Multi-AZ DB
- **Engine**: MySQL/PostgreSQL
- **Multi-AZ**: Enabled
- **VPC Name**: ScalableAppVPC-vpc
- **Public Access**: No
- **Security Group**: Same as EC2
- **Connect**: Use RDS Endpoint in app code

![RDS](../aws-images/RDS-DB.png)


## 9. Web Page - Testing the Application

Accessed via the ALB DNS name

![Web Page](../aws-images/WebPage.png)
