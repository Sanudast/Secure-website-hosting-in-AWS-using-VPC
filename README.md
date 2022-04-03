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
3. Creating private and public route tables and associate the subnets.
4. Creating Internte gateway for public subnet and adding it to public route table.
5. Creating Nat gateway for private subnet and adding it to private route table.
6. Creating 3 Ec2 instance (Bastion instance, Website Instance, Database Instance).


1- Creating a VPC with CIDR subnet

Login into your AWS management console and search VPC on the top navbar. Then select VPC from the drop-down.
From the VPC dashboard, click on your VPC's and click on Create VPC. Give the VPC a name and provide a IPV4 CIDR block.

![image](https://user-images.githubusercontent.com/100775801/161413193-2e5e7c5d-347f-493d-acd4-2be085d203e7.png)

Here am providing VPC name as my-vpc-123 and 172.16.0.0/16 as IPV4 CIDR block. Also you can add extra tags from Tags section.

![image](https://user-images.githubusercontent.com/100775801/161413318-bfb31a24-691e-482f-8253-0221ac94fd12.png)

