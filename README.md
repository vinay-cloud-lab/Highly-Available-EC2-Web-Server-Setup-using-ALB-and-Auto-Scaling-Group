# Project: Highly Available EC2 Web Server Setup using ALB and Auto Scaling Group

## What This Project Includes
- Launch Template
- Auto Scaling Group (ASG)
- Target Group
- Application Load Balancer (ALB)
- Apache Web Server
- Security Groups
- Health Checks

## Step-by-Step Explanation

# 1. Create a Launch Template
- Choose Amazon Linux 2 (or Ubuntu)
- Add User Data to install web server

# Example User Data (Nginx)
#!/bin/bash
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hello from Auto Scaling Instance" > /var/www/html/index.html

- Select instance type (t3.micro)
- Save the template

# 2. Create a Target Group
- Target type: Instances
- Protocol: HTTP (80)
- Health check path: /
- Create the target group

# 3. Create an Application Load Balancer (ALB)
- Type: Application Load Balancer
- Scheme: Internet-facing
- Select at least 2 public subnets
- Security Group: Allow HTTP (80) from anywhere
- Listener: Forward traffic to the Target Group

# 4. Create an Auto Scaling Group (ASG)
- Select the Launch Template
- Choose at least 2 subnets
- Attach the Target Group
- Set capacities:
  - Desired: 1
  - Minimum: 1
  - Maximum: 3

# Optional Scaling Policies

## Up-Scaling Policy
- Type: Target Tracking or Simple Scaling
- Rule: Scale OUT when CPU >= 60%
- Action: Add 1 instance

## Down-Scaling Policy
- Rule: Scale IN when CPU <= 30%
- Action: Remove 1 instance


# 5. Configure Security Groups
## ALB Security Group:
- Allow HTTP (80) from anywhere

## EC2 Instance Security Group:
- Allow HTTP (80) from ALB SG only
- Allow SSH only from your IP (optional)

# 6. Testing the Setup
- Copy ALB DNS name → Open in browser
- You should see your Apache page

## Test Auto Scaling:
- Stop or terminate an EC2 instance
- ASG launches a new one automatically

## Test Load Balancing:
- Refresh browser multiple times
- Requests should hit different EC2 instances

## Why This Project Is Useful
- Shows deep EC2 knowledge
- High availability architecture
- Understanding load balancing
- Proper use of security groups
- Health check configuration
- Real-world cloud engineer skills
