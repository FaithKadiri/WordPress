Deploying a WordPress website on AWS using a three-tier architecture

![three-tier](https://github.com/user-attachments/assets/9c2d6919-7f2b-4a45-8fff-9bc4d19853e7)



The three-tier approach ensures the website is scalable and secure. The archiecture includes the database layer, application layer and web layer.
To start, we will create a VPC with CIDR block 173.31.0.0/16 in our preferred region.
Set up public subnets in two availability zones, attach an Internet Gateway (IGW), create a route table (RTB), and configure routing (0.0.0.0/0 → IGW). Associate public subnets with this RTB and enable auto-assign public IPv4 addresses.

Set up private subnets in the same availability zones, create a RTB, and associate private subnets. Deploy a NAT Gateway (NATGW) in a public subnet, allocate an Elastic IP, and update the RTB (0.0.0.0/0 → NATGW).

Create six security groups (SGs):

ALB SG: allows HTTP/HTTPS traffic.

Instance Connect Endpoint SG: for secure connection to private instances.

SSH SG: It is security best practice to create a separate SSH SG. We will configure the inbound rule to accept traffic from the ICE SG

Application Server SG: allows traffic from ALB, SSH from ICE SG.

Database SG (DB-SG): This SG only allows MySQL connections (port 3306) from the Application Server SG.

EFS SG: Elastic File System (EFS) will use this SG to accept NFS traffic from our Application Servers.

Test AWS Instance Connect Endpoint (ICE) to connect to private instances without SSH.

Configure the database layer: Create an RDS MySQL database, define a DB subnet group, disable public access, assign the DB-SG, and disable backups.

Set up EFS: Create a file system, associate it with private subnets, and apply the EFS SG.

Launch an EC2 instance in a private subnet, associate it with the Application Server SG, and connect using ICE.
