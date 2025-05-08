---
title: "Task assigned by Bhavan"
datePublished: Thu May 08 2025 18:08:49 GMT+0000 (Coordinated Universal Time)
cuid: cmafola32000q09l57bbr06hh
slug: task-assigned-by-bhavan

---

## Task 1 : Login through Username/Password instead of Key Pairs

* To check whether we can login to our Amazon Linux EC2 instance using username and password.
    
* Yes , we can do that by removing the key pair parameter from cft and adding user data in ec2 that set username and password and also enables password authentication in SSH config.
    
* `UserData:`
    
    `Fn::Base64: !Sub |`
    
    `#!/bin/bash`
    
    `echo "ec2-user:AkshatSecurePassword123" | chpasswd`
    
    `sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config`
    
    `sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/' /etc/ssh/sshd_config`
    
    `systemctl restart sshd`
    
* Login using public ip “ssh ec2-user@54.91.21.28” and it will ask for password.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746720078678/e5ab41db-b60b-4fa1-a1df-1385416eb0d8.png align="center")
    

## Task 2 : Can we ref already existing resources in CFT

* Yes we can ref a already existing resource in CFT, we have to provide it in parameters and can set default value to that resource .
    
* Example: We are creating a ec2 instance using CFT and giving key-pair as parameter.
    
* `Parameters:`
    
    `KeyPairName:`
    
    `Type: AWS::EC2::KeyPair::KeyName`
    
    `Description: Name of an existing EC2 KeyPair for SSH access`
    
    `Default: ak-key`
    
    `Resources:`
    
    `EC2Instance:`
    
    `Type: AWS::EC2::Instance`
    
    `Properties:`
    
    `ImageId: ami-0b0dcb5067f052a63`
    
    `InstanceType: t2.micro`
    
    `KeyName: !Ref KeyPairName`
    
* Here first we are creating a parameter of key-pair , then add ref to our ec2 instance.
    

## Task 3 : Can we pass CIDR of Vpc in parameters

* We have defined VPC in the template , can we define its CIDR in parameters.
    
* Yes we define the CIDR in parameters and ref it to the VPC in same template.
    
* `Parameters:`
    
    `VpcCidr:`
    
    `Type: String`
    
    `Default: 10.0.0.0/16`
    
    `Description: CIDR block for the VPC`
    
    `Resources:`
    
    `MyVPC:`
    
    `Type: AWS::EC2::VPC`
    
    `Properties:`
    
    `CidrBlock: !Ref VpcCidr`
    
    `EnableDnsSupport: true`
    
    `EnableDnsHostnames: true`
    
    `Tags:`
    
    `- Key: Name`
    
    `Value: my-vpc`
    
* Here we are defining the VPC cidr in parameter and ref it to VPC creation under resources.
    

## Task 4 : Do CFT have variables ?

* No CFT do not have true variables.
    
* But we can use parameters (user defined input) as variables.