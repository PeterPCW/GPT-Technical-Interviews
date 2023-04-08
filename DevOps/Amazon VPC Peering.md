# What is Amazon VPC? Explain the process of creating a VPC and subnet in AWS. Provide an example of a VPC peering configuration.

Amazon Virtual Private Cloud (VPC) is a web service that enables you to create and manage a logically isolated network environment within the AWS cloud. It allows you to launch Amazon Elastic Compute Cloud (EC2) instances, Amazon Relational Database Service (RDS) instances, and other resources in a virtual network that you define.

To create a VPC in AWS, you need to follow these steps:

1. Sign in to the AWS Management Console and navigate to the VPC dashboard.
2. Click on the "Create VPC" button.
3. Enter a name for your VPC, and select the IPv4 CIDR block that you want to assign to the VPC.
4. Choose the tenancy type for your VPC. You can select either "Default" or "Dedicated".
5. Click on the "Create" button to create your VPC.

To create a subnet in your VPC, you need to follow these steps:

1. Sign in to the AWS Management Console and navigate to the VPC dashboard.
2. Click on the "Create Subnet" button.
3. Select the VPC that you want to create the subnet in.
4. Enter a name for your subnet, and select the IPv4 CIDR block that you want to assign to the subnet.
5. Select the availability zone for your subnet.
6. Click on the "Create" button to create your subnet.

VPC peering is a method of connecting two VPCs together to enable resources in one VPC to communicate with resources in another VPC using private IP addresses. To configure VPC peering, you need to follow these steps:

1. Sign in to the AWS Management Console and navigate to the VPC dashboard.
2. Click on the "Create Peering Connection" button.
3. Enter a name for your peering connection, and select the VPC that you want to peer with.
4. Click on the "Create Peering Connection" button to create your peering connection.
5. Accept the peering connection request in the other VPC.
6. Add a route to the route table in each VPC to enable traffic to flow between the two VPCs.