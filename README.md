# VPC-Secure-website-hosting-using-Ec2


## Description

In this article, I will hep you to host a website with more security using AWS VPC and Ec2 service.
Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.


## Prerequisite

AWS account with management console access and full access to VPC and EC2 services.

## Diagram

![main drawio](https://user-images.githubusercontent.com/100775801/161412579-0ebf57d5-fa4c-4067-b765-5cb6ad4537f0.png)


## Table of Contents

1. Creating a VPC with CIDR subnet
2. Creating 2 public and 1 private subnet.
3. Enabling auto-assign public IPv4 address public subnets. 
4. Creating Internet gateway and attaching to VPC
5. Creating public route tables and adding Internet gateway entry.
6. Creating NAT gateway for private subnet
7. Creating private route table and adding NAT gateway entry. Adding private subnet to route table association
8. Creating security group for Bastion server.
9. Creating security group for wordpress server.
10. Creating security group for database server.
11. Creating 3 Ec2 instance (Bastion instance, Website Instance, Database Instance).


# 1- Creating a VPC with CIDR subnet

Login into your AWS management console and search VPC on the top navbar. Then select VPC from the drop-down.
From the VPC dashboard, click on your VPC's and click on Create VPC. Give the VPC a name and provide a IPV4 CIDR block. 

![image](https://user-images.githubusercontent.com/100775801/161413193-2e5e7c5d-347f-493d-acd4-2be085d203e7.png)

Here am providing VPC name as my-vpc-123 and 172.16.0.0/16 as IPV4 CIDR block. Also you can add extra tags from Tags section.

![image](https://user-images.githubusercontent.com/100775801/161413318-bfb31a24-691e-482f-8253-0221ac94fd12.png)

After creating the VPC enabling the DNS hostname for the VPC.

![image](https://user-images.githubusercontent.com/100775801/161413706-f4243a6a-058c-4ec3-b0b7-f5fd75142591.png)



# 2- Creating 2 public and 1 private subnet

Go to subnets section in VPC console, from there we can create subnets for our VPC.  Click on Create subnet,

![image](https://user-images.githubusercontent.com/100775801/161413527-2cfb2f9f-e240-4068-923f-e07fc93f59da.png)

First we need to select our created VPC from the list box. Then we are creating 2 subnets for public and 1 subnet for private, also selecting availability zones.
Here am naming the public subnets as public-subnet-1 and public-subnet-2, private subnet as private-subnet-1.  Now we need to subnet the CIDR block, I'm using subnet calulatore for this. URL: https://www.calculator.net/ip-subnet-calculator.html?cclass=b&csubnet=18&cip=172.16.0.0%2F16&ctype=ipv4&printit=0&x=83&y=14
you can refer the link for more informtaion about the subneting.

![image](https://user-images.githubusercontent.com/100775801/161413979-246a335f-764d-42eb-9345-374474132806.png)
![image](https://user-images.githubusercontent.com/100775801/161413989-9975682a-35da-4dbb-98ff-b2c4060f5256.png)
![image](https://user-images.githubusercontent.com/100775801/161414002-7b8b1c67-a3bb-4feb-a5c8-be1bf8858d3c.png)
![image](https://user-images.githubusercontent.com/100775801/161414016-e37bff81-5259-4afe-80ca-ceb5058d0b6c.png)
![image](https://user-images.githubusercontent.com/100775801/161414033-64f48b15-98a5-4fee-9142-ed58a759acac.png)

Subnet creation completed.


# 3- Enabling auto-assign public IPv4 address public subnets

We need public IP for our public subnets. From subnets section we can enable the auto-assign public IPv4 address. For that select the public subnets and click edit subnet settings. 

![image](https://user-images.githubusercontent.com/100775801/161414264-755fef9f-1c55-447f-92cb-2dc437a593c5.png)

Then enable auto-assign public IPv4 and save the settings.
![image](https://user-images.githubusercontent.com/100775801/161414329-5ec8f9f2-f039-42a6-a2ca-d552078e1785.png)


# 4- Creating Internte gateway and attaching to VPC

Next we are going to create internet gateway. For that go to the internet gateway section and click on create internte gateway.
![image](https://user-images.githubusercontent.com/100775801/161414507-12d20e1a-7795-4965-ae80-af434c7f1b81.png)

Here am provided vpcproject-ig as internet gateway name and creating the IG.

![image](https://user-images.githubusercontent.com/100775801/161414558-92b9d0ab-721d-4777-a014-f82ccbc17176.png)

After creating the Internet gateway we are attaching it to our VPC. For that select the Internet gateway and click on attach to VPC in actions.

![image](https://user-images.githubusercontent.com/100775801/161414636-695a1396-a957-42a0-8682-27051831aaf5.png)

Then select the VPC and click attach internet gateway.

![image](https://user-images.githubusercontent.com/100775801/161414678-f62782a7-3ef1-45c5-bc17-084c4e2536bb.png)


# 5- Creating public route tables and adding Internet gateway entry

Now we are going to create a route table for public subnet and adding the internet gateway entry. Adding internet gateway in VPC for the communication with outside world. Go to route tables section, here we can see a deafult route table of our VPC. I'm using this route table as public route table. So am editing the name of the default route table to public-rtbl.

![image](https://user-images.githubusercontent.com/100775801/161415322-9292eecb-2221-482f-ad99-511e405269b3.png)

After that adding the internet gateway entry to the route table. 

![image](https://user-images.githubusercontent.com/100775801/161415357-6ea105c1-6da8-4564-8aed-c1975749aef6.png)

Now all the subnet are associated with public-rtbl. 

# 6- Creating NAT gateway for private subnet

NAT gateway is used to communicate a instance in private subnet to outside world. For creating NAT gateway go to the NAT gateway section and click create NAT gateway.

![image](https://user-images.githubusercontent.com/100775801/161415562-8e4b6bc3-cb10-46a0-b2e9-ff02edbbb553.png)

The Nat gateway should be created in a public-subnet so am selecting on of the created public-subnet and provide the name vpcproject-nat for nat gateway.
Next we need to assign a elastic IP to nat gateway we can do that from the same section. After selecting all these settings create the NAT gateway.

![image](https://user-images.githubusercontent.com/100775801/161415699-ac21a3f5-0663-4898-a45c-15d0a0167730.png)

Once the status of the NAT gateway changed from Pending to Available the creation will complete.

# 7-Creating private route table and adding NAT gateway entry. Adding private subnet to route table association

Next we are going to create a private route table for private subnet. Click the create route table from route table section.
![image](https://user-images.githubusercontent.com/100775801/161415880-aacd9469-74dd-4bc1-a39d-d6cc8301fc31.png)

I'm providing private-rtbl as route table name and select the VPC.
![image](https://user-images.githubusercontent.com/100775801/161415950-cb0c8231-4b87-47d9-8c71-8743f41fffc2.png)

Adding the Nat gateway to the route table.

![image](https://user-images.githubusercontent.com/100775801/161416046-7c956fa5-94b9-44d2-b376-1a557e071284.png)


Next we are adding the private subnet to the private route table. For that select the route table and in action section select edit subnet association.

![image](https://user-images.githubusercontent.com/100775801/161416101-dea75cf3-b326-47b6-92e6-0982092ca87c.png)

Select the private subnet and click save associations

![image](https://user-images.githubusercontent.com/100775801/161416131-3d769082-9083-4552-8910-467fa752c4b6.png)


# 8-Creating security group for Bastion server

## Bastion server
A bastion is a special purpose server instance that is designed to be the primary access point from the Internet and acts as a proxy to your other EC2 instances.

As you can see in the diagram, we are creating a bastion server only for SSH to wordpress and database server. The SSH access to bastion server is only allowed from our IP. So SSH connections from other IPs are not allowed by the baston security group. 
We can create the security group from security group section. Click create security group,

Here am providing bastion-sg as security group name and selecting VPC. We can add a short description in description section. 

![image](https://user-images.githubusercontent.com/100775801/161416865-4c8d2ca7-f47c-4c32-88a5-5b44ecb780c7.png)

Now we are adding the SSH allow only from my IP rule in inbound rules and creating the security group.

![image](https://user-images.githubusercontent.com/100775801/161416913-afe5ee72-b8f3-4ca9-a332-67e9f5ffb619.png)


# 9-Creating security group for wordpress server

In this security group we are only allowing SSH connection from servers which are using security group "bastion-sg". Also allowing 80,443 connections from all IPv4 and IPv6. Here am providing wordpress-sg as security group name.

![image](https://user-images.githubusercontent.com/100775801/161417133-4c03687d-ec7f-4b21-99cd-4e2b951fc0bd.png)

Inbound rules

![image](https://user-images.githubusercontent.com/100775801/161417179-1a8373c1-ff6e-40d3-9100-c844b3d853f0.png)

# 10-Creating security group for database server

In this security grop we are only allow SSH connections from servers which are using security group "bastion-sg". Also allowing 3306 connections from servers which are using security group "wordpress-sg". Here am providing databases-sg as security group name.

![image](https://user-images.githubusercontent.com/100775801/161417327-13e85bc7-e830-45fb-9839-37623a99ad13.png)

Inbound rules

![image](https://user-images.githubusercontent.com/100775801/161417361-d6d57c0c-6319-472c-a867-7e2517936e17.png)


# 11-Creating 3 Ec2 instance (Bastion instance, Website Instance, Database Instance).

Now we are going to create 3 instance. Select Ec2 from the drop-down. From the Ec2 dashboard, select instances section and click on Launch instances.

## Bastion Instance

Now we are going to create instance for Bastion server.

![image](https://user-images.githubusercontent.com/100775801/161418277-a77837e7-f1dc-4b22-90c6-36c1997d6790.png)

Select any Os as per our requirement. 

![image](https://user-images.githubusercontent.com/100775801/161418304-2903d2a5-27bc-4022-86be-110611823d13.png)

Choose an Instance Type.

![image](https://user-images.githubusercontent.com/100775801/161418344-0483bea7-5b1b-454d-899d-7d94505bec76.png)

Select the correct VPC from the list. choose a public subnet
![image](https://user-images.githubusercontent.com/100775801/161418423-1ed442c8-c00f-4355-896a-5e98891b0b52.png)

Adding Name tag as Bastion
![image](https://user-images.githubusercontent.com/100775801/161418448-7c42643b-a1b6-48c7-bd62-a0d083da8577.png)

Selecting the security group "bastion-sg" which we are already created for bastion server.
![image](https://user-images.githubusercontent.com/100775801/161418483-bb3bd04d-c4a8-436d-8ed6-7c2b724f9381.png)

Choose any of the key-pair and click launch instance.
![image](https://user-images.githubusercontent.com/100775801/161418521-8c59b573-8ff2-4380-b145-5f2a46a81154.png)


## Website Instance

Now we are going to create instance for website server.
We are going to create the website instance in public subnet and adding the group "wordpress-sg"
Follow the previous steps to create instance.

![image](https://user-images.githubusercontent.com/100775801/161418621-1c3d8200-1f61-4d12-ac42-b826b036c645.png)
![image](https://user-images.githubusercontent.com/100775801/161418641-d01a5685-6942-4c86-a0a8-995fa7bb9a4f.png)
![image](https://user-images.githubusercontent.com/100775801/161418649-5defae7f-06d7-45dd-9e85-d0a47d74eaf5.png)


## Database Instance

Now we are going to create instance for database server.
We are going to create the database instance in private subnet and adding the group "database-sg"
Follow the previous steps to create instance.

![image](https://user-images.githubusercontent.com/100775801/161418779-4e7722fc-a69e-4a6e-899e-aa9195dfbecd.png)
![image](https://user-images.githubusercontent.com/100775801/161418798-2de1acb9-d7f7-4ec2-9f03-2906ea3aef44.png)
![image](https://user-images.githubusercontent.com/100775801/161418814-062f8271-d427-41dd-89f1-c56f000e2206.png)


























