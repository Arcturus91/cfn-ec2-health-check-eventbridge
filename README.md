# AWS EventBridge Architecture with EC2, ALB and Lambda

This CloudFormation template deploys a highly available web application infrastructure with automated monitoring and notifications.

## Architecture Overview

![Architecture Diagram]

- VPC with public subnets across multiple AZs
- Application Load Balancer (ALB) for traffic distribution
- Auto Scaling EC2 instances running Apache
- EventBridge monitoring for EC2 state changes
- Lambda function for event processing
- SNS notifications for state changes

## Prerequisites

- AWS Account
- Permissions to create:
  - VPC and networking components
  - EC2 instances and ALB
  - IAM roles
  - Lambda functions
  - EventBridge rules
- SNS topics with a subscribed email

## Infrastructure Components

### Networking

- VPC (10.0.0.0/16)
- 2 Public Subnets
- Internet Gateway
- Route Tables

### Security

- ALB Security Group (Port 80 inbound)
- EC2 Security Group (Port 80 from ALB)
- IAM Roles for EC2, Lambda, and EventBridge

### Compute

- Auto Scaling Group (2-4 instances)
- t2.micro instances with Apache
- Application Load Balancer
- Health checks and target groups

### Monitoring

- EventBridge rule for EC2 state changes
- Lambda function for event processing
- SNS notifications

## Deployment

1. Clone this repository
2. Deploy by uploading this yml file to cloudformation or using AWS CLI:

```bash
aws cloudformation create-stack \
  --stack-name event-bridge-demo \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_IAM
```

## Testing the Deployment

1. Verify Apache Installation (you would need to add instance connect security group permission):

```bash
sudo systemctl status httpd
```

2. Check the DNS provided in the ALB and make a GET request to it. You should get HTML for a response.

3. Terminate your instance and review if you got the email notification.

## Monitoring and Logs

### Apache Status

- Service status: `systemctl status httpd`

### CloudWatch Logs

- Lambda function logs
- ALB access logs
- VPC Flow Logs

## Security Considerations

1. Network Security

   - Public subnets for ALB only
   - EC2 instances accept traffic only from ALB
   - Security group rules follow least privilege

2. Access Management
   - IAM roles with minimal required permissions
   - EC2 instance profile for AWS service access
   - Lambda execution role for SNS publishing

## Troubleshooting

1. HTTP Access Issues

   - Verify Apache is running
   - Check security group configurations
   - Validate ALB health checks
   - Review target group settings

2. Notification Issues
   - Check Lambda execution logs
   - Verify SNS topic permissions
   - Validate EventBridge rule configuration

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## Author

Arturo Barrantes

## Reference

- AWS CloudFormation documentation
- Apache HTTP Server documentation
- AWS EventBridge documentation
