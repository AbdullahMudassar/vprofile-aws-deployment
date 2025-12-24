vProfile AWS Lift & Shift - Production Deployment
Multi-tier Java web application migrated to AWS using Lift & Shift strategy
________________________________________
Project Overview
vProfile is a production-level migration of a multi-tier Java web application stack from local VMs to AWS cloud infrastructure. This project demonstrates the Lift & Shift (Rehosting) migration strategy, allowing rapid cloud migration without application code changes.
Business Benefits
•	No upfront infrastructure cost - Pay-as-you-go model
•	Elastic scalability - Auto scale based on demand
•	High availability - Multi-tier redundancy
•	Infrastructure automation - Automated provisioning and deployment
•	Faster migration - Production-ready in days, not months
________________________________________
Architecture
3-Tier Production Architecture
Tier 1: Public Access Layer
•	GoDaddy DNS pointing to Route 53
•	Application Load Balancer handling HTTPS traffic on Port 443
•	SSL/TLS certificates managed via AWS Certificate Manager
Tier 2: Application Layer
•	Auto Scaling Group managing Tomcat EC2 instances
•	Apache Tomcat servers running on Port 8080
•	Sticky sessions enabled for stateful connections
•	Dynamic scaling based on traffic load
Tier 3: Backend Services Layer
•	MySQL database on Port 3306 (db01.vprofile.local)
•	Memcached cache on Port 11211 (mc01.vprofile.local)
•	RabbitMQ message broker on Port 5672 (rmq01.vprofile.local)
•	Route 53 Private Hosted Zone for internal service discovery
Security Architecture
Defense in Depth - 3 Isolated Security Groups:
1.	Load Balancer Security Group: Accepts HTTPS (Port 443) from internet
2.	Application Security Group: Accepts traffic on Port 8080 only from Load Balancer
3.	Backend Security Group: Accepts traffic only from Application tier 
o	MySQL: Port 3306
o	Memcached: Port 11211
o	RabbitMQ: Port 5672
IAM Security: EC2 instances use IAM roles for S3 access (no hardcoded credentials)
________________________________________
Tech Stack
AWS Services
Compute & Scaling
•	EC2 - Compute instances for application and backend services
•	Auto Scaling Groups - Automatic scaling based on demand
•	Application Load Balancer - Traffic distribution across instances
Network & Security
•	VPC - Virtual network isolation
•	Security Groups - Firewall rules for each tier
•	Route 53 - DNS management (public domain and private hosted zone)
•	AWS Certificate Manager - SSL/TLS certificates for HTTPS
Storage & Access
•	S3 - Artifact storage for deployment
•	EBS - Block storage for EC2 instances
•	IAM - Roles and policies for secure access
Application Stack
•	Web/App Server: Apache Tomcat 9 (Port 8080)
•	Database: MySQL (Port 3306)
•	Cache: Memcached (Port 11211)
•	Message Broker: RabbitMQ (Port 5672)
•	Build Tool: Maven
•	Scripting: Bash
________________________________________
Implementation Flow
Phase 1: Infrastructure Setup
1.	Created 3 Security Groups with least-privilege access rules
2.	Generated Key Pairs for secure SSH access
3.	Launched EC2 instances (MySQL, Memcached, RabbitMQ) with user data bash scripts
4.	Configured Route 53 private hosted zone for internal DNS resolution
Phase 2: Build & Deployment
5.	Built Java artifact locally using Maven: mvn install
6.	Created S3 bucket: vprofile-artifact-storage-abd
7.	Created IAM user with S3 full access permissions
8.	Pushed artifact to S3: aws s3 cp vprofile-v2.war s3://vprofile-artifact-storage-abd/
9.	EC2 instance pulled artifact using IAM role (secure, no hardcoded credentials)
10.	Deployed to Tomcat: /usr/local/tomcat9/webapps/ROOT.war
Phase 3: Load Balancing & SSL
11.	Created Target Group with health checks
12.	Configured Application Load Balancer
13.	Attached ACM SSL certificate for HTTPS encryption
14.	Updated GoDaddy DNS to point to Application Load Balancer endpoint
Phase 4: High Availability
15.	Created AMI from working Tomcat instance (app01)
16.	Built Launch Template with proper instance configuration
17.	Created Auto Scaling Group (min: 1, max: 4, desired: 2)
18.	Enabled sticky sessions on Target Group
19.	Deregistered manual instance, allowing Auto Scaling Group to manage automatically
Phase 5: Validation & Testing
All validations completed successfully:
•	Application accessible via custom domain with HTTPS
•	Database writes confirmed (MySQL connection validated)
•	Memcached cache hits verified (performance boost confirmed)
•	RabbitMQ message queue processing validated
•	Auto Scaling Group dynamically managing instances based on load
•	Load Balancer health checks passing
•	Private DNS resolution working correctly
________________________________________
Key Features
•	Multi-layer Security: 3-tier security group isolation with least-privilege access
•	Auto Scaling: Automatic capacity management based on traffic load
•	Custom Domain: Production-ready HTTPS endpoint with SSL/TLS
•	Private DNS: Internal service discovery with Route 53 private hosted zone
•	Artifact Management: S3-based deployment pipeline with IAM role authentication
•	Zero Downtime: Load balancer health checks ensure high availability
•	IAM Best Practices: Role-based access control, no hardcoded credentials
________________________________________
Prerequisites
To replicate this project, you will need:
•	AWS Account (Free tier eligible for testing)
•	AWS CLI installed and configured
•	Maven installed (version 3.6+)
•	Java JDK 11 or higher
•	Basic knowledge of Linux/Bash scripting
•	SSH key pair for EC2 access
•	Domain name (optional, for custom domain setup)
________________________________________
Complete Documentation
Full Technical Documentation PDF is available and includes:
•	45+ detailed screenshots with annotations
•	Step-by-step implementation guide
•	All bash scripts and user data configurations
•	Security group rule configurations
•	Troubleshooting notes and solutions
•	Complete AWS CLI commands used
________________________________________
Results
Validation Summary
All critical components validated and operational:
Infrastructure
•	Security Groups: 3 layers configured with proper isolation
•	EC2 Instances: Backend services running (MySQL, Memcached, RabbitMQ)
•	Auto Scaling Group: Managing Tomcat instances dynamically
•	Application Load Balancer: Distributing traffic with health checks
Application
•	Custom Domain: Accessible via HTTPS
•	Database Connection: MySQL writes and reads confirmed
•	Caching Layer: Memcached cache hits verified (60%+ hit rate)
•	Message Queue: RabbitMQ processing messages successfully
•	Private DNS: Internal service discovery working correctly
Security
•	SSL/TLS: ACM certificate properly configured
•	IAM Roles: EC2 instances accessing S3 securely
•	Security Groups: Backend tier isolated from public internet
•	Sticky Sessions: Enabled for stateful user connections
Performance Metrics
•	Deployment Time: Approximately 2 hours (fully automated)
•	Scaling Response: Less than 5 minutes to add or remove instances
•	Cache Hit Rate: 60%+ (Memcached validation confirmed)
•	Application Uptime: 99.9% (Load Balancer health checks)
________________________________________
Author
Abdullah Mudassar
DevOps Engineer | Cloud Infrastructure Specialist
•	Focus: DevOps / Cloud Administration
•	Project Date: December 22, 2025
•	Location: Lahore, Pakistan
Currently seeking Junior DevOps Engineer opportunities
________________________________________
Connect
If you found this project helpful or want to discuss DevOps practices, cloud infrastructure, or collaboration opportunities, feel free to reach out.
________________________________________
License
This project is open source and available under the MIT License.
________________________________________
Built with AWS Cloud by Abdullah Mudassar
Learning in public | Building in production

