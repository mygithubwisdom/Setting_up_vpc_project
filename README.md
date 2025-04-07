AWS VIRTUAL PRIVATE CLOUD (VPC) Setup - KCVPC


OVERVIEW 

This project Virtual Private Cloud (VPC) with both public and private subnets, configured internet connectivity via an Internet Gateway and NAT Gateway, set up the necessary route tables, and secure the network with security groups and network ACLs. Conclude by deploying EC2 instances to verify the setup.


Architecture Diagram
![Screenshot (1051)](https://github.com/user-attachments/assets/b0b5f483-d691-4ba7-b718-0f937cf42995)


1. Create a VPC : VPC Name: KCVPC IPv4 CIDR Block: 10.0.0.0/16

2. Create Subnets
Public Subnet : IPv4 CIDR Block: 10.0.1.0/24 Availability Zone:EU-West-1A (Ireland) region

Private Subnet : IPv4 CIDR Block: 10.0.2.0/24 Availability Zone:EU-West-1B (Ireland) region


3. Configured an Internet Gateway (IGW)
    Created and attached to KCVPC

4. Configure Route Tables
   Public Route Table:
   Name: PublicRouteTable
   Associate PublicSubnet with this route table.
   routes internet traffic to the IGW (0.0.0.0/0 -> IGW).

Private Route Table
   Name: PrivateRouteTable
   Associate PrivateSubnet with this route table.
   Routes internet traffic (0.0.0.0/0) through NAT Gateway.

5. Configure NAT Gateway  
    Created in PublicSubnet with an Elastic IP.
    Allows private instances to access the internet.
   
6. Set Up Security Groups
   Security Group for Public Instances (e.g., Web Servers)

Allow HTTP (80) and HTTPS (443) from anywhere IPV4 Ip (0.0.0.0/0).
Allow SSH (22) from a specific IP.

   Security Group for Private Instances (e.g., Database Servers)
   Allow MySQL (3306) access from PublicSubnet.

7. Configure Network ACLs (NACLs)
Public Subnet NACL
Setup to allow inbound HTTP, HTTPS, and SSH traffic.
Allows outbound traffic.

Private Subnet NACL
Allowed inbound traffic from the public subnet. 
Allowed outbound traffic to the public subnet and internet. 


8. Deploy Instances EC2
   Public Instance: Deployed in PublicSubnet, accessible via SSH.
   Private Instance: Deployed in PrivateSubnet, can access the internet through NAT.




Creating these subnets splits your network into segments that can be used for different types of resources.
This step enables Internet access to and from resources in your Public Subnet.

After setting up the NAT Gateway, update the Private Route Table to route all internet-bound traffic (0.0.0.0/0) to the NAT Gateway. This setup allows resources in the PrivateSubnet to access the Internet indirectly.

Route tables control the flow of traffic in your network; here you segregate public and private traffic flows.
Security GRoup
Security groups act as virtual firewalls to control traffic to and from your instances.

Network
Network ACLs provide an additional layer of security at the subnet level to complement your security groups.
