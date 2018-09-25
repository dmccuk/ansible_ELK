# ansible Playbook & Role

one ansible playbook and ansible role to install, configure and run ELK Stack + beats

## List out available Indexes:

    # curl localhost:9200/_cat/indices?v

![Alt text](terraform_472.png?raw=true)
# Getting started with Terraform

In my example, we are going to setup and launch one AWS EC2 instance. Please read the Pre-requisites below and make sure you are happy to proceed.
---
## Pre-Requisites:

1. Install Terraform. [link](https://www.terraform.io/intro/getting-started/install.html)
2. Have an account on AWS (free Tier if possible). [link](https://aws.amazon.com/free)
3. Some basic knowledge of AWS.
  * Creating and download your key pair (.pem file). [link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
    * Create your Access key and access secret (one time creation). [link](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey)
  * Familiarity with the AWS console.
  * AWS training - I recommend Ryan Kroonemburg on Udemy. [link](https://www.udemy.com/user/ryankroonenburg/)

## Setup instructions

### On your server (where you have installed Terraform)

Create /opt/demo:
````
# mkdir -p /opt/demo
# cd /opt/demo
````
