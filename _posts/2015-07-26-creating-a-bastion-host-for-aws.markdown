---
layout: post
title: Creating a Bastion Host for AWS
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-07-26T20:19:57-05:00
---

At my company, security is a primary concern due to the type of data that we work with on a daily basis. We use AWS for all of our staging and production infrastructure. AWS provides many excellent security features by default for access to server instances, but the AWS support team recommended that we take SSH access to our system a step further. 

Bastion Host: 
*A special purpose computer on a network specifically designed and configured to withstand attacks.*

In the context of SSH for AWS, a bastion host is a server instance itself that you must SSH into before you are able to SSH into any of the other servers in your VPC. 

Here are the steps I took to set one up:

**Step 1:** Create an EC2 instance on AWS. I choose a micro sized instance since it is on the free-tier and it's only purpose is to access other servers.

**Step 2:** Create a security group for the Bastion host that opens up port 22 for SSH and select "My IP" as the source. This is so that only you and other teammates that add their IP's have access to the Bastion. 

**Step 3:** Change the security groups of existing instances so that any inbound SSH is only accessible via the Bastion host's IP address. 

**Step 4:** Edit your local ~/.ssh/config file and add the following: 
<script src="https://gist.github.com/vaughanj10/14cc728f6c8db7072cad.js"></script> Where hostname is the IP address of the bastion host and username is the one that use to log into the server. 

This allows you to ssh into your Bastion server by just typing in `ssh bastion` from the command line. Also, you can see that there is a line above that says `Forward Agent yes` . This sets up SSH forwarding from your local machine to the Bastion Host so that the .pem file you use to access your EC2 instances will be made available when you try to connect to your servers.

**Step 5:** 
SSH into the Bastion Host and then try to SSH into any of your existing AWS server instances and voilà! You now have a more secure way of getting into your servers because they are now only accessible from the Bastion. 


Sources:
I used this [guide here](http://blogs.aws.amazon.com/security/post/Tx3N8GFK85UN1G6/Securely-connect-to-Linux-instances-running-in-a-private-Amazon-VPC) for help on best practices for a SSH Bastion Host and could be useful for those setting up ssh-agent on a Mac or Windows machine.

Cheers!