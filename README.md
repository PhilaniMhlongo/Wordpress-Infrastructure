# WordPress AWS Infrastructure Deployment

This guide outlines how to deploy a highly available WordPress infrastructure using AWS CloudFormation. The infrastructure includes a multi-tier VPC, EC2 instances for WordPress, RDS for the database, EFS for shared storage, and an ALB for load balancing.

---

## Prerequisites

1. **AWS Account**: Ensure you have an active AWS account.
2. **AWS CLI**: Install and configure the AWS CLI with appropriate credentials. Refer to the [AWS CLI Documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) for setup instructions.
3. **CloudFormation Templates**: Ensure the following templates are available in your working directory:
   - `NjomaneIndustriesVPC.yaml`
   - `Infrastructure.yml`

---

## Deployment Steps

### 1. Deploy the VPC Stack

The VPC stack sets up the foundational network infrastructure required for the WordPress application.

Run the following command:

```bash
aws cloudformation create-stack \
  --stack-name NjomaneIndustriesVPC \
  --template-body file://NjomaneIndustriesVPC.yaml \
  --capabilities CAPABILITY_IAM \
  --profile iamadmin-philani
```

### 2. Monitor VPC Stack Progress

Wait for the VPC stack to complete. Check the status with:

```bash
aws cloudformation describe-stacks \
  --stack-name NjomaneIndustriesVPC \
  --profile iamadmin-philani
```

Ensure the status is `CREATE_COMPLETE` before proceeding.

---

### 3. Deploy the WordPress Infrastructure Stack

Once the VPC is successfully deployed, deploy the WordPress infrastructure:

```bash
aws cloudformation create-stack \
  --stack-name Infrastructure \
  --template-body file://Infrastructure.yaml \
  --parameters \
    ParameterKey=DBPassword,ParameterValue=Philani#Wethinkcode023 \
    ParameterKey=DBRootPassword,ParameterValue=Philani#Wethinkcode023 \
  --capabilities CAPABILITY_IAM \
  --profile iamadmin-philani
```

### 4. Monitor Infrastructure Stack Progress

Check the status of the WordPress infrastructure stack using:

```bash
aws cloudformation describe-stacks \
  --stack-name Infrastructure \
  --profile iamadmin-philani
```

---

## Features of the Deployed Infrastructure

### Architecture

- **VPC**: Multi-tier architecture with public, application, and database subnets spanning three Availability Zones for high availability.
- **WordPress Application Layer**:
  - EC2 instances in Auto Scaling Groups (ASG) behind an Application Load Balancer (ALB) for scaling and redundancy.
  - Elastic File System (EFS) for shared storage across WordPress instances.
- **Database**:
  - Amazon RDS for MySQL with high availability and automated backups.
- **Monitoring & Security**:
  - CloudWatch for instance monitoring and alarms.
  - IAM roles for least privilege access.
  - Security groups and network ACLs for fine-grained access control.

---

## Notes

- **Database Passwords**: Update the `DBPassword` and `DBRootPassword` parameters with secure values before deployment.
- **Monitoring**: Use the AWS Management Console or CLI to monitor resources and adjust scaling policies as needed.
- **Costs**: Ensure you understand AWS pricing for the resources deployed.

---

## Troubleshooting

- **Stack Creation Errors**:
  - Check CloudFormation events for error details:
    ```bash
    aws cloudformation describe-stack-events \
      --stack-name <Stack-Name> \
      --profile iamadmin-philani
    ```
- **Resource Limits**: Ensure your AWS account has sufficient limits for VPCs, EC2 instances, and other resources.
- **Logs**: Check CloudWatch logs for debugging application or infrastructure issues.

---
