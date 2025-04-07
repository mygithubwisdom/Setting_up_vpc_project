#AWS VIRTUAL PRIVATE CLOUD (VPC) Setup - KCVPC


**OVERVIEW **

This project Virtual Private Cloud (VPC) with both public and private subnets, configured internet connectivity via an Internet Gateway and NAT Gateway, set up the necessary route tables, and secure the network with security groups and network ACLs. Conclude by deploying EC2 instances to verify the setup.


**Architecture Diagram**

![Screenshot (1051)](https://github.com/user-attachments/assets/dd6701c3-b52b-404d-a725-62f12be1770d)



1. **Create a VPC :**  
   VPC Name: KCVPC  
   IPv4 CIDR Block: 10.0.0.0/16

2. **Create Subnets**  
   **Public Subnet :**  
   IPv4 CIDR Block: 10.0.1.0/24  
   Availability Zone:EU-West-1A (Ireland) region

   **Private Subnet :**  
   IPv4 CIDR Block: 10.0.2.0/24  
   Availability Zone:EU-West-1B (Ireland) region

3. **Configured an Internet Gateway (IGW)**  
   Created and attached to KCVPC

4. **Configure Route Tables**  
   **Public Route Table:**  
   Name: Public Route Table  
   Associate Public Subnet with this route table.  
   routes internet traffic to the IGW (0.0.0.0/0 -> IGW).

   **Private Route Table**  
   Name: Private Route Table  
   Associate Private Subnet with this route table.  
   Routes internet traffic (0.0.0.0/0) through NAT Gateway.

5. **Configure NAT Gateway**  
   Created in Public Subnet with an Elastic IP.  
   Allows private instances to access the internet.

6. **Set Up Security Groups**  
   **Security Group for Public Instances (e.g., Web Servers)**  
   Allow HTTP (80) and HTTPS (443) from anywhere IPV4 Ip (0.0.0.0/0).  
   Allow SSH (22) from a specific IP.

   **Security Group for Private Instances (e.g., Database Servers)**  
   Allow MySQL (3306) access from PublicSubnet.

7. **Configure Network ACLs (NACLs)**  
   **Public Subnet NACL**  
   Setup to allow inbound HTTP, HTTPS, and SSH traffic.  
   Allows outbound traffic.

   **Private Subnet NACL**  
   Allowed inbound traffic from the public subnet.  
   Allowed outbound traffic to the public subnet and internet.

8. **Deploy Instances EC2**  
   Public Instance: Deployed in PublicSubnet, accessible via SSH.  
   Private Instance: Deployed in PrivateSubnet, can access the internet through NAT.

# MY AWS VPC Deployment Guide ad Screeshots

   I have Designed and set up a Virtual Private Cloud (VPC) with both public and private subnets implementing routing , security groups and network access control lists (NACLs) to ensure proper communication and security within the VPC.

## 1. Create the VPC
- Navigate to **AWS Console → VPC → Create VPC**.  
- Set **Name** as `KCVPC`, **CIDR Block**: `10.0.0.0/16`.  
- Click **Create**.
**VPC Screeshot**
![Screenshot (1047)](https://github.com/user-attachments/assets/897f1248-00c3-4cb3-97b7-43211a71a85a)

## 2. Create Public and Private Subnets
- Navigate to **Subnets → Create Subnet**.  
- Select **VPC**: `KCVPC`.  
- **PublicSubnet:**  
  - CIDR Block: `10.0.1.0/24`  
  - Availability Zone: `eu-west-1a`  
- **PrivateSubnet:**  
  - CIDR Block: `10.0.2.0/24`  
  - Availability Zone: `eu-west-1b`  
Creating these subnets splits your network into segments that can be used for different types of resources.
![Screenshot (1024)](https://github.com/user-attachments/assets/64001a1e-2a50-487e-af0b-86a47ce89980)
---

## 3. Attach Internet Gateway
- Navigate to **Internet Gateway → Create IGW**.  
- Name it `KCVPC-IGW`.  
- Attach it to `KCVPC`.  
This step enables internet access to and from resources in your Public Subnet.

---

## 4. Configure Route Tables

### Public Route Table
- Create `PublicRouteTable` and associate `PublicSubnet`.  
- Add a route: `0.0.0.0/0 → IGW`.

### Private Route Table
- Create `PrivateRouteTable` and associate `PrivateSubnet`.  
- Add a route: `0.0.0.0/0 → NAT Gateway`.  

Route tables control the flow of traffic in your network; here you segregate public and private traffic flows.
![Screenshot (1020)](https://github.com/user-attachments/assets/a18b4efe-4c11-460c-b371-73fe28e96afb)

---

## 5. Create and Attach NAT Gateway
- Allocate an **Elastic IP**.  
- Create **NAT Gateway** in `PublicSubnet`.  
- Attach the **Elastic IP**.  

Update the `PrivateRouteTable` to route all internet-bound traffic (`0.0.0.0/0`) to the NAT Gateway. This setup allows resources in the Private Subnet to access the Internet indirectly.
![Screenshot (1019)](https://github.com/user-attachments/assets/dbef2faf-7f1e-4485-905a-65a0e51f65a5)


---

## 6. Configure Security Groups

### Public Security Group (`PublicSG`)
- Allow **HTTP (80)** and **HTTPS (443)** from anywhere.  
- Allow **SSH (22)** from your IP.

### Private Security Group (`PrivateSG`)
- Allow **MySQL (3306)** traffic from `PublicSubnet`.
- Attach VPC to each security group.

Security groups act as virtual firewalls to control traffic to and from your instances.
![Screenshot (1022)](https://github.com/user-attachments/assets/d6419c4f-bc73-4a15-8293-9f12a6c5a926)
![Screenshot (1023)](https://github.com/user-attachments/assets/c7ac496d-c992-4829-a4e9-c0aaefcb1e95)
![Screenshot (1021)](https://github.com/user-attachments/assets/8d1b15ca-1818-46df-9302-ae89177bc680)




---

## 7. Configure Network ACLs
- **PublicNACL:** Allow HTTP, HTTPS, and SSH.  
- **PrivateNACL:** Allow traffic from `PublicSubnet`.  

Network ACLs provide an additional layer of security at the subnet level to complement your security groups.
![Screenshot (1024)](https://github.com/user-attachments/assets/28682118-9f9b-4e5f-a9e5-84067fd65877)

![Screenshot (1027)](https://github.com/user-attachments/assets/e299383c-f9f7-4e29-a87a-5eff3be56436)



---

## 8. Launch EC2 Instances

### Public EC2 Instance
- Launch an instance in `PublicSubnet` using `PublicSG`.  
- Verify **SSH access**.

### Private EC2 Instance
- Launch an instance in `PrivateSubnet` using `PrivateSG`.  
- Verify **NAT Gateway access**.

---![Screenshot (1048)](https://github.com/user-attachments/assets/e430c09d-ac05-46de-bf59-917934be08a1)


## Outcome
I have successfully configured a two web servers with AWS Ec2 for the Public and Private subnets, and my networking setup is complete.
The Private and Public Network access control lists (NACL) were successfully created. I successfully configured a NAT Gateway too. The NAT Gateway forwards traffic from the Ec2 instance and MYSql Database in the private subnet to the internet and then sends the response back to the EC2 instance. The route table for private subnets was updated to point internet traffic to the NAT gateway. The NAT allows my Private IP of my Private subnet to be reachable. Because everything remains local within the VPC. The private subnet stays local and can only be reached within the the VPC.


**Author: Wisdom Ugwoh*

##Lucid chart link for diagram

**https://lucid.app/publicSegments/view/2e53b168-c09d-4676-bc08-4d007cb9886a/image.png**


