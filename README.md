![Alt text](3._Host_a_Dynamic_Web_App_on_AWS.png)
---

# Hosting a Dynamic Website on AWS

## Overview

This project demonstrates how to host a dynamic website on AWS, utilizing a variety of AWS services to ensure high availability, scalability, and security. The infrastructure was designed to leverage AWS's capabilities to provide a robust and resilient web application deployment.

## Architecture

The architecture of the project includes the following components:

1. **VPC Configuration**: A Virtual Private Cloud (VPC) was configured with both public and private subnets that spanned across two different availability zones to enhance fault tolerance and high availability.
   
2. **Internet Gateway**: An Internet Gateway was deployed to enable connectivity between the VPC instances and the internet.
   
3. **Security Groups**: Security Groups were established to serve as a network firewall mechanism, controlling traffic to and from the web servers.
   
4. **Multi-AZ Deployment**: The infrastructure leveraged two Availability Zones to increase system reliability and fault tolerance.
   
5. **Public Subnets**: Public Subnets were utilized for infrastructure components like the NAT Gateway and Application Load Balancer.
   
6. **EC2 Instance Connect Endpoint**: Implemented for secure connections to assets within both public and private subnets.
   
7. **Private Subnets**: Web servers (EC2 instances) were placed within Private Subnets for enhanced security.
   
8. **NAT Gateway**: Allowed instances in both the private Application and Data subnets to access the Internet via the NAT Gateway.
   
9. **EC2 Instances**: Hosted the dynamic website on EC2 Instances within private subnets.
   
10. **Application Load Balancer**: Employed an Application Load Balancer and a target group to distribute web traffic evenly to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
   
11. **Auto Scaling Group**: Utilized an Auto Scaling Group to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.
   
12. **SSL Certificate**: Secured application communications using AWS Certificate Manager.
   
13. **SNS Notifications**: Configured Simple Notification Service (SNS) to alert about activities within the Auto Scaling Group.
   
14. **Route 53 for DNS**: Registered the domain name and set up a DNS record using Route 53.
   
15. **S3 for Application Code Storage**: Used Amazon S3 to store the application code, enabling easy access and version control.

## Deployment Script

Below is the script used to deploy the dynamic website, including the setup of the database using Flyway for migration management:

```bash
#!/bin/bash

S3_URI=s3://simba-sql-files/V1__shopwise.sql
RDS_ENDPOINT=dev-rdb-db.cbkei0ek6xig.us-east-2.rds.amazonaws.com
RDS_DB_NAME=applicationdb
RDS_DB_USERNAME=will
RDS_DB_PASSWORD=Kobeisgoat24

# Update all packages
sudo yum update -y

# Download and extract Flyway
sudo wget -qO- https://download.red-gate.com/maven/release/com/redgate/flyway/flyway-commandline/10.17.0/flyway-commandline-10.17.0-linux-x64.tar.gz | tar -xvz 

# Create a symbolic link to make Flyway accessible globally
sudo ln -s `pwd`/flyway-10.17.0/flyway /usr/local/bin

# Create the SQL directory for migrations
sudo mkdir sql

# Download the migration SQL script from AWS S3
sudo aws s3 cp "$S3_URI" sql/

# Run Flyway migration
flyway -url=jdbc:mysql://"$RDS_ENDPOINT":3306/"$RDS_DB_NAME" \
  -user="$RDS_DB_USERNAME" \
  -password="$RDS_DB_PASSWORD" \
  -locations=filesystem:sql \
  migrate
```

## Project Repository

The reference diagram, deployment scripts, and additional documentation for this project can be found in the GitHub repository: [(https://github.com/simbaaws88/Host-a-Dynamic-Web-App-on-AWS)](https://github.com/simbaaws88/Host-a-Dynamic-Web-App-on-AWS/edit/main/README.md)

## Conclusion

This project showcases a comprehensive deployment of a dynamic website on AWS, ensuring high availability, scalability, security, and ease of management through various AWS services. The use of automation and best practices in cloud architecture ensures that the website is resilient and ready for production workloads.

---
