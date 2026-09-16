# EC2 CloudWatch Monitoring

### Overview

A hands-on AWS project where I configured an Amazon EC2 instance to collect and send system logs for a WordPress application using the CloudWatch Agent.

The project was completed while studying AWS through guided hands-on training and reproducing the setup in my own AWS environment.


### AWS Services Used

- Amazon EC2
- AWS IAM
- AWS Systems Manager (SSM)
- Amazon CloudWatch



### Architecture

EC2 -> Cloudwatch Agent -> Cloudwatch Logs -> Log Group
						|
						-Log Stream



### What Was Configured

- Created an EC2 instance
- Attached an IAM role to the EC2 instance with the required managed policies:
  - `CloudWatchAgentServerPolicy`
  - `AmazonSSMFullAccess`
- Installed the CloudWatch Agent
- Configured the CloudWatch Agent to collect:
  - `/var/log/access.log`
  - `/var/log/error.log`
- Saved the CloudWatch Agent configuration to SSM
- Configured CloudWatch log storage
- Verified that logs from the EC2 instance were being sent to CloudWatch Logs



### Things I Learned

- What IAM permissions the EC2 instance requires to interact with CloudWatch and SSM
- How to configure the CloudWatch Agent
- How logs are sent from an EC2 instance to CloudWatch Logs
- The difference between a CloudWatch Log Group and Log Stream 



