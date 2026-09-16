# CloudFormation

## Purpose

The CloudFormation template contains infrastructure that I previously configured through hands-on AWS labs. It was used both to practice infrastructure automation and to reduce the time required to repeatedly create and delete the environment due to billing concerns.

The course instructor provided the CloudFormation template containing the necessary infrastructure. I used it to recreate an environment that I had already worked with manually. I have hands-on experience with the AWS components provisioned by the template.


## Infrastructure Provisioned

- VPC
- Internet Gateway
- Route table
- Web, App, DB and Reserved subnets
- EC2 instance
- Security Group
- WordPress application components
- Supporting AWS resources

## Why It Was Useful

Using CloudFormation significantly reduced the time required to manually configure and delete the infrastructure. It also reduced the amount of repeated manual work and allowed me to practice deploying and removing AWS infrastructure through automation while limiting unnecessary AWS usage and potential charges.


## Learning Notes

- Learned how AWS infrastructure can be represented as code instead of being created entirely through the AWS Console.
- how CloudFormation resources reference and depend on other resources within the same template.
- how a single template can create multiple related AWS resources as part of one environment.
- how CloudFormation can be used to repeatedly create and remove an environment for hands-on experimentation.
- the importance of understanding the underlying AWS resources before automating them with CloudFormation.


## Source

This infrastructure was implemented as part of guided AWS training and reproduced in my own AWS environment for hands-on practice.
