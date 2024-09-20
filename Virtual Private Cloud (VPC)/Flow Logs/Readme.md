# AWS VPC Flow Logs

## Overview
VPC Flow Logs capture information about the IP traffic going to and from network interfaces in your VPC. You can create a flow log for a VPC, a subnet, or a network interface. Flow logs help you troubleshoot connectivity issues, monitor network traffic, and perform security analysis.

### Key Features:
- **Traffic Monitoring**: Capture details on accepted and rejected IP traffic.
- **Troubleshooting**: Identify sources and reasons for failed or rejected traffic.
- **Security and Compliance**: Analyze traffic logs to detect anomalies and ensure compliance with organizational policies.
- **Granularity**: You can choose to log at the VPC, subnet, or specific interface level.

## Flow Log Components:
- **Destination**: Flow log data can be published to CloudWatch Logs, S3, or a third-party monitoring solution via Lambda.
- **Fields**: Logs include fields like source IP, destination IP, protocol, action (accept/reject), and bytes transferred.

## Use Cases:
1. **Network Performance Monitoring**: Track the health and performance of your VPC network.
2. **Security Auditing**: Identify suspicious traffic patterns and security threats.
3. **Cost Optimization**: Analyze the volume and nature of traffic to optimize the usage of resources.

## How to Create a Flow Log

### Using AWS Console:
1. Go to the **VPC** dashboard.
2. Select the **VPC**, **subnet**, or **network interface** for which you want to enable flow logs.
3. Choose **Create Flow Log** and configure the log destination (CloudWatch Logs, S3).
4. Select the **log format** and **filters** (e.g., accept or reject traffic).
5. Review and create the flow log.

### Using AWS CLI:
```bash
aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-12345678 --traffic-type ALL --log-group-name my-flow-logs --deliver-logs-permission-arn arn:aws:iam::123456789012:role/FlowLogsRole
```

## Flow Log Cost

### Key Pricing Components:
- **Data Ingestion**: Flow logs are typically charged based on the volume of data ingested (measured in GB).
- **Destination Costs**:
  - **CloudWatch Logs**: Additional costs apply for storing logs in CloudWatch.
  - **S3 Storage**: If logs are sent to S3, you'll incur S3 storage costs.
  - **Data Transfer**: If logs are sent across regions or accounts, data transfer fees may apply.

### Pricing Example:
- **CloudWatch Logs**: $0.50 per GB of ingested data (for the first 10 TB/month).
- **S3 Storage**: Standard S3 storage pricing applies, starting at $0.023 per GB for the first 50 TB per month.

For up-to-date pricing, visit the [AWS Pricing Page](https://aws.amazon.com/pricing/).

## AWS Free Tier for VPC Flow Logs
- VPC Flow Logs **themselves** are not directly covered under the AWS Free Tier.
- However, the data ingestion and storage (via CloudWatch or S3) can incur costs.
  - **S3 Free Tier**: 5 GB of standard storage is free per month.
  - **CloudWatch Logs Free Tier**: 5 GB of data ingestion and 5 GB of archived log data per month are included in the Free Tier.

Be mindful that any data exceeding these free tier limits will incur charges.

## Flow Log Formats:
Flow logs are written in JSON format and include fields such as:
- `version`: Log version
- `account-id`: AWS account ID
- `interface-id`: Network interface ID
- `srcaddr`: Source IP address
- `dstaddr`: Destination IP address
- `srcport`: Source port
- `dstport`: Destination port
- `protocol`: IANA protocol number
- `packets`: Number of packets transferred
- `bytes`: Number of bytes transferred
- `action`: Whether traffic was accepted or rejected
- `log-status`: Log creation status (e.g., success or failure)

## Best Practices:
- **Filter Traffic**: You can log all traffic or focus on specific types (e.g., accepted traffic only).
- **Rotate Logs**: Set up log retention policies in CloudWatch or S3 to manage storage costs.
- **Analyze Logs**: Use AWS Athena or third-party tools to query and analyze log data for insights.

---
