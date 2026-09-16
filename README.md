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


### How It Works

The WordPress application runs on an EC2 instance. The CloudWatch Agent runs on the instance and is configured to collect the application's access and error logs.

The agent uses the IAM permissions attached to the EC2 instance using Instance profile to interact with AWS services. 

The CloudWatch Agent configuration is stored in AWS Systems Manager Parameter Store.

The collected logs are sent to Amazon CloudWatch Logs, where they are stored in a Log Group and organized into Log Streams.


### IAM Configuration

The EC2 instance was assigned an IAM role to allow the instance and CloudWatch Agent to interact with the required AWS services.

The role included:

- `CloudWatchAgentServerPolicy`
- `AmazonSSMFullAccess`

`CloudWatchAgentServerPolicy` was used to provide the permissions required by the CloudWatch Agent.

`AmazonSSMFullAccess` was used because the CloudWatch Agent configuration was stored and retrieved through AWS Systems Manager Parameter Store.


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



### Verification

The setup was tested by generating and accessing the logs on the EC2 instance and Checking CloudWatch logs to verify that the configured log files were being collected successfully.

#### What I Tested

- Verified that the CloudWatch Agent was running on the EC2 instance.
- Verified that the configured log files were being collected.
- Verified that the Cloudwatch config was in SSM.
- Verified that the logs were appearing in CloudWatch Logs.
- Verified the Log Group and Log Stream created for the collected logs.


### AWS Environment

- EC2 instance running the WordPress application
- CloudWatch Agent installed on the instance
- CloudWatch Agent configuration stored in AWS Systems Manager Parameter Store
- CloudWatch Log Group used to store the collected logs



### Why CloudFormation

The AWS environment was initially configured manually to understand how the VPC, subnets, routing, security groups, EC2 and application components work together.
CloudFormation was then used to automate the infrastructure so the environment could be deployed repeatedly for hands-on experimentation without having to recreate each component manually every time.
The environment was deleted after practice to avoid leaving chargeable AWS resources running unnecessarily.


#### Infrastructure Components


##### VPC

- Custom VPC with CIDR range `10.16.0.0/16`
- IPv6 addressing is enabled through an Amazon-provided IPv6 CIDR block

##### Subnets 

- The VPC contains separate Web, App, DB, and Reserved subnet tiers.
- Each tier has subnets deployed across three Availability Zones.
- The Web subnets are configured for public connectivity, while the App and DB tiers are intended for the backend portion of the application.
- Multiple subnets for each tier are distributed across Availability Zones so the infrastructure is not concentrated in a single Availability Zone.

##### Routing

- The Web subnets are associated with a route table containing a default route (`0.0.0.0/0`) through the Internet Gateway.
- This allows resources in the Web subnets to send traffic toward the internet.

##### IGW 

- The Internet Gateway is attached to the vpc.
- The Internet Gateway provides the VPC with a path to and from the internet for resources in subnets whose routing allows internet connectivity.

##### SG

- Allows inbound SSH traffic on port `22`.
- Allows inbound HTTP traffic on port `80`.
- SSH is permitted from `0.0.0.0/0` for IPv4 and `::/0` for IPv6 in the template.

##### SSM

- AWS Systems Manager Parameter Store is used to store the CloudWatch Agent configuration.
- The configuration can then be retrieved by the CloudWatch Agent on an instance instead of maintaining the configuration only as a local file.

##### EC2 Instance 

- The EC2 instance runs in a Web subnet.
- CloudFormation configures the instance and installs the application components required for WordPress.
- WordPress is configured to use MariaDB as its database.
- The instance serves HTTP traffic through port `80`.
- `cowsay` was used as an additional check that the CloudFormation configuration completed successfully.

##### IAM

- An IAM role is attached to the EC2 instance through an instance profile.
- The role provides the permissions required for software running on the instance to interact with AWS services.
- In this setup, the permissions are used for interaction with Systems Manager and CloudWatch.


##### Cloudwatch 

- The CloudWatch Agent runs on the EC2 instance and collects configured system/application log files.
- The collected logs are sent to Amazon CloudWatch Logs for centralized monitoring and storage.


