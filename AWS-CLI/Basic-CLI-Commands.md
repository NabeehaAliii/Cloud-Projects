As a beginner DevOps engineer, mastering the AWS CLI (Command Line Interface) is essential for automating tasks, managing resources, and ensuring efficient cloud operations. Here's a detailed guide on important AWS CLI commands you should know:

 # Basic AWS CLI Commands

## 1. **S3 Commands** (Simple Storage Service)

### List all S3 buckets:
```bash
aws s3 ls
```

### Create a new S3 bucket:
```bash
aws s3 mb s3://my-bucket-name
```

### Upload a file to a bucket:
```bash
aws s3 cp path/to/local/file.txt s3://my-bucket-name/
```

### Download a file from a bucket:
```bash
aws s3 cp s3://my-bucket-name/file.txt path/to/local/
```

### Sync a local directory with an S3 bucket:
```bash
aws s3 sync /local/directory s3://my-bucket-name/
```

---

## 2. **EC2 Commands** (Elastic Compute Cloud)

### List all EC2 instances:
```bash
aws ec2 describe-instances
```

### Launch a new EC2 instance:
```bash
aws ec2 run-instances --image-id ami-12345678 --instance-type t2.micro --key-name MyKeyPair --security-groups my-sg
```

### Stop an EC2 instance:
```bash
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
```

### Start an EC2 instance:
```bash
aws ec2 start-instances --instance-ids i-1234567890abcdef0
```

### Terminate an EC2 instance:
```bash
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

### Create a key pair (for SSH access):
```bash
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```

---

## 3. **IAM Commands** (Identity and Access Management)

### List all IAM users:
```bash
aws iam list-users
```

### Create a new IAM user:
```bash
aws iam create-user --user-name new-user
```

### Attach a policy to a user:
```bash
aws iam attach-user-policy --user-name new-user --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
```

### Create an access key for a user:
```bash
aws iam create-access-key --user-name new-user
```

---

## 4. **VPC Commands** (Virtual Private Cloud)

### List all VPCs:
```bash
aws ec2 describe-vpcs
```

### Create a new VPC:
```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

### Create a subnet:
```bash
aws ec2 create-subnet --vpc-id vpc-12345678 --cidr-block 10.0.1.0/24
```

### Create an internet gateway:
```bash
aws ec2 create-internet-gateway
```

### Attach an internet gateway to a VPC:
```bash
aws ec2 attach-internet-gateway --vpc-id vpc-12345678 --internet-gateway-id igw-12345678
```

---

## 5. **EBS Commands** (Elastic Block Store)

### List all EBS volumes:
```bash
aws ec2 describe-volumes
```

### Create a new EBS volume:
```bash
aws ec2 create-volume --availability-zone us-east-1a --size 10
```

### Attach an EBS volume to an instance:
```bash
aws ec2 attach-volume --volume-id vol-12345678 --instance-id i-1234567890abcdef0 --device /dev/sdf
```

---

## 6. **CloudWatch Commands** (Monitoring and Logging)

### List all CloudWatch alarms:
```bash
aws cloudwatch describe-alarms
```

### Create a CloudWatch alarm:
```bash
aws cloudwatch put-metric-alarm --alarm-name "HighCPUUtilization" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanOrEqualToThreshold --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:123456789012:my-sns-topic
```

### Get CloudWatch metrics:
```bash
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --start-time 2022-01-01T00:00:00 --end-time 2022-01-01T01:00:00 --period 300 --statistics Average
```

---

## 7. **Route53 Commands** (DNS Management)

### List hosted zones:
```bash
aws route53 list-hosted-zones
```

### Create a hosted zone:
```bash
aws route53 create-hosted-zone --name example.com --caller-reference 2022-01-01-01
```

### List DNS records:
```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC
```

---

## 8. **RDS Commands** (Relational Database Service)

### List all RDS instances:
```bash
aws rds describe-db-instances
```

### Create a new RDS instance:
```bash
aws rds create-db-instance --db-instance-identifier mydbinstance --allocated-storage 20 --db-instance-class db.t2.micro --engine mysql --master-username admin --master-user-password mypassword --backup-retention-period 3
```

### Delete an RDS instance:
```bash
aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
```

---

## 9. **SNS Commands** (Simple Notification Service)

### Create an SNS topic:
```bash
aws sns create-topic --name my-topic
```

### Subscribe an email to a topic:
```bash
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol email --notification-endpoint myemail@example.com
```

### Publish a message to a topic:
```bash
aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message "Hello, World!"
```

---

### Conclusion

Mastering these commands will give you a strong foundation for managing AWS resources via the command line, helping you automate, configure, and monitor your cloud infrastructure efficiently as a beginner DevOps engineer.

