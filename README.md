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
4. Creating Internte gateway and attaching to VPC
5. Creating private and public route tables and associate the subnets.
6. Creating Internte gateway for public subnet
7. Creating Nat gateway for private subnet and adding it to private route table.
8. Creating 3 Ec2 instance (Bastion instance, Website Instance, Database Instance).


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























