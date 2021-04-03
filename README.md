# AWS CloudFormation Template- 3 tier VPC 
The CloudFormation template for configuring a public-facing three-tier web application VPC across multiple Availability Zones.. 

# Infrastructure as Code (IaC)
A template for repeated creation of the same stack or to be used as a foundation to start a new stack..

# VPC or subnet IP ranges

|  Item  | CIDR Range   |  Usable IPs | Description  |   |
|---|---|---|---|---|
| VPC  |10.1.0.0/16   |65,536   | The whole range used for the VPC and all subnets  |   |   |
|Public SubnetA    | 10.1.10.0/24  | 251  | The public subnet in the first Availability Zone  |   |
|Public SubnetB    | 10.1.20.0/24  | 251  | The public subnet in the second Availability Zone  |   |
|Private SubnetA    | 10.1.50.0/24  | 251  | The private subnet in the first Availability Zone  |   |
|Private SubnetB    | 10.1.60.0/24  | 251  | The private subnet in the second Availability Zone  |   |
|   |   |   |   |   |

# Design Feature/Details

- The design is basically for a public-facing three-tier web application in a VPC across multiple Availability Zones. 
-The CF template creates a Multi-AZ, multi-subnet VPC infrastructure with managed NAT gateways in the public subnet for each Availability Zone.
  -The VPC is given a CIDR of 10.1.0.0/16.
  -The design has four subnets, two public and two private across two AZs.
  -NAT GW provides egress for the private subnets.
-Enabled Internet access to VPC by adding resources for Internet Gateway and Gateway Attachment. 
-The VPC can be completely detached from the public internet by using VPN connections or AWS Direct Connet to connect exclusively to on-premises DC. 
-Each subnet in the VPC is associated with a routing table that will control that subnet's routing. 
-We can use network ACLs as firewalls to control inbound and outbound traffic at the subnet level I, f needed later.  
-By creating a Security Group, we could allow inbound rules for LB, App, and DB instances. 
-The private route table makes use of the NAT gateway to access the internet. This route sends Internet-bound traffic  (Destination CIDR Block: 0.0.0.0/0) to the NAT gateway (!Ref NATGateway). In turn, the NAT will send it out to the Internet Gateway due to its placement in the public subnet. Response traffic received by the NAT is forwarded to the instance that made the request. 
   
       NB: - The design is for a plane VPC network to hold a three-tier application.
           - I have not added steps for 
               - building EC2 instances with the needed AMIs, 
               - building ALB, Internal LB 
               - and other options that may be needed in an actual production environment.

# Network Diagram