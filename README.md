# VPC-Secure-website-hosting-using-Ec2


## Description

In this article, I will hep you to host a website with more security using AWS VPC and Ec2 service.
Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.


## Prerequisite

AWS account with management console access and full access to VPC and EC2 services.

## Diagram

![main drawio](https://user-images.githubusercontent.com/100775801/161412579-0ebf57d5-fa4c-4067-b765-5cb6ad4537f0.png)


## Table of Contents

1. Creating a VPC with 2 public and 1 private subnet.
2. Creating private and public route tables and associate the subnets.
3. Creating Internte gateway for public subnet and adding it to public route table.
4. Creating Nat gateway for private subnet and adding it to private route table.
5. Creating 3 Ec2 instance (Bastion instance, Website Instance, Database Instance).
