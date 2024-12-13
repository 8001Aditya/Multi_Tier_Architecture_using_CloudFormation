## Multi-Tier Architecture using AWS CloudFormation

This repository contains a CloudFormation template to deploy a three-tier architecture on AWS. The setup includes a web tier, an application tier, and a database tier with proper networking and security configurations.

### Features:
1. **VPC Setup:**
   - Creates a VPC with public and private subnets.
   - Public subnet hosts the web tier, while private subnets host the application and database tiers.

2. **Web Tier:**
   - Launches an EC2 instance in a public subnet.
   - Security group allows HTTP (port 80) and SSH (port 22) traffic from the internet.

3. **Application Tier:**
   - Launches an EC2 instance in a private subnet.
   - Security group allows SSH (port 22) access from the web tier.

4. **Database Tier:**
   - Launches an Amazon RDS MySQL instance in a private subnet.
   - Security group allows MySQL traffic (port 3306) only from the application tier.
   - Database is retained when the stack is deleted.

5. **Route 53 Integration:**
   - Configures a hosted zone and maps a domain name to the web tier's EC2 instance.

### Prerequisites:
- AWS account with IAM permissions to create resources.
- Key pair for SSH access to EC2 instances.
- Update the template with a valid AMI ID and domain name.

### Deployment Steps:
1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. Validate the template:
   ```bash
   aws cloudformation validate-template --template-body file://cloudformation_test_env.yaml
   ```

3. Deploy the stack:
   ```bash
   aws cloudformation create-stack --stack-name MultiTierStack --template-body file://cloudformation_test_env.yaml --parameters ParameterKey=KeyName,ParameterValue=<your-key-pair-name>
   ```

4. Monitor stack creation:
   ```bash
   aws cloudformation describe-stacks --stack-name MultiTierStack
   ```

### Outputs:
- **WebInstancePublicIP:** Public IP of the web server.
- **RDSInstanceEndpoint:** Endpoint of the MySQL database.

### Cleanup:
To delete the stack (RDS instance will not be deleted):
```bash
aws cloudformation delete-stack --stack-name MultiTierStack
