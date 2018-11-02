# Welcome to AWS - EC2 Management

This tutorial is about using Amazon Web Services (AWS) Command Line Interface (awscli) for launching, starting, stopping instances and the related tasks to do so.

Related tasks:
* Managing Identities to your AWS account (awscli) 
* Creating and Authenticating AWS EC2 security groups (awscli)
* Notes on VPCs (can be awscli)
	* IP/CIDR notation and some open questions

Steps to this tutorial:
1. Authentication Management
2. Instance Management
3. Notes on infrastructure for parallel computing / MPI

## 1. Authentication Management 

This has 2 parts. The first is about using the AWS console to create identity groups. The second half is about setting up a security group and key pairs to actually create instances and determine who how someone can access AWS resources.

#### 1A. Identity and Access Management (IAM) for EC2 Resources [WIP]

This is pretty valuable general knowledge to have about AWS. It's pretty straightforward, it's just the process of setting up different logins to your account's resources, and setting up permissions for those users. For ease of management, you can assign different logins, or "identities", to different identity groups and manage their permissions all at once. Every identity will have a special username, as well as a corresponding "Special Access Key". Their login will be different than root access login because sub-users will need to have the Account ID of their superuser (the main account user).


#### 1B. Security Groups and Key-Pair for Creation of EC2 Instances 
First, you want to create an admin/root identity for your AWS account. You should be using this at all times to engage in AWS management console.

Then you want to create identity groups with permissions to start instances. And like test them out. Honestly, not sure if you even want to give out this permission because they can charge directly to your account by running instances. Especially p2.xlarges. Well, actually, if you only have 1 p2.xlarge (or whatever instance type being used to perform deep learning, and your identity group can only start p2 instances, then it's not a big deal). 

To create a security group from which to access your instance, you can run the following:
```bash
aws ec2 create-security-group --group-name [name of group] --description "[desc]"
```

Now, you must set the permissions for that security group. This can be done by authorizing the previous security group to access your instances on your default VPC (network of EC2 instances) to be accessed via SSH. This means setting the access protocol to tcp, and allowing access over port 22 (reserved for SSH).

```bash
aws ec2 authorize-security-group-ingress --group-id [id of group spit out by previous command] --protocol tcp --port 22 --cidr 0.0.0.0/0
```

A note on the command above. This will allow any instance accessible by your security group to be SSH'd into from ***ANY*** IP address. Which may not be the right policy for your team, especially if its only a few people that need access to your resources. This can be further addressed by adding more security layers in a VPC (partially covered later).

Then create a key-pair for ec2, and distribute the key to your identity-group.
```bash
aws ec2 create-key-pair --key-name [name your key] --query 'KeyMaterial' --output text > [key name].pem
# On linux, you must also change permissions so only you can view the key
chmod 400 [key name].pem
```

And now you can spread your pem file around to those who you wish to have access to your EC2 instance. Once a instance is launched with a public IP address, it can be accessed with the created key like so:
```bash
ssh -i [key name].pem ubuntu@PUB.LIC.IP.ADDRESS
```
The -i flag is called the identity flag for SSH access to a given server. For more complicated SSH calls, such as port forwarding access to a Jupyter notebook, see the Bash crash-course file.

## 2. Instance Management - Planned Completion

Configure your awscli profile to be making queries from the us-west-2 region (US West - Oregon). It's where the p2.xlarges are located.

Launching and starting an instance. 

```bash
aws ec2 run-instances --image-id [AMI id] --count 1 --security-groups-id "[security group id]" --key-name [name of your key pair (do not include .pem)] --instance-type p2.xlarge
# for conda deep learning enviroment, use --image-id ami-3b6bce43
```

Starting instances from instance ids.

```bash
aws ec2 start-instances --instance-ids "[instance-ids]"
```

Finding instance ids.
```bash
aws ec2 describe-instances | grep "InstanceId"
```

Stopping an instance.
```bash
aws ec2 stop-instances --instance-ids "[instance=ids]"
```

## 3. Parallel Computing Infrastructure

You're going to want to specify your own Virtual Private Cloud (VPC) via AWS. And specify your instances to be a part of this VPC, or on the same subnet. This will require specificying an internet gateway to your VPC, as well as specs to allow your instances on the subnet to ping each other. More information, [look here](https://treyperry.com/2015/06/22/ipv4-cidr-vpc-in-a-nutshell/).

More to the point. Maybe also need MPI? It stands for [Message Passing Interface](http://mpitutorial.com/tutorials/launching-an-amazon-ec2-mpi-cluster/). It's a protocol(?) developed by MIT researchers for cluster computing. I don't know how it works. I don't know what types of instances to use with it, and I haven't tried the commands listed in the link (above).
