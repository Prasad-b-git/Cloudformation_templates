# AWS Lambda S3 Notification System

This project implements an automated notification system using AWS Lambda, S3, and SNS. When a file is uploaded to the specified S3 bucket, a Lambda function is triggered to send an email notification via SNS to subscribed email addresses.

## Architecture Overview

The system consists of:
- S3 Bucket with versioning enabled
- Lambda Function for processing upload events
- SNS Topic for email notifications
- IAM Roles and Permissions
- Lambda Permission for S3 trigger

## Features

* Automated email notifications for S3 uploads
* File versioning enabled on S3 bucket
* Serverless architecture using AWS Lambda
* Event-driven design with S3 triggers
* Secure IAM role configurations
* Email notifications through SNS

## Prerequisites

1. AWS Account with appropriate permissions
2. AWS CLI installed and configured
3. Basic understanding of:
   * AWS CloudFormation
   * AWS Lambda
   * Amazon S3
   * Amazon SNS
   * IAM Roles and Policies

## Resources Created

### 1. S3 Bucket
- Name: year24082025
- Features:
  * Versioning enabled
  * Lambda trigger configuration
  * Automatic event notification

### 2. Lambda Function
- Runtime: Python 3.9
- Memory: 128MB
- Timeout: 10 seconds
- Environment Variables:
  * SNS_TOPIC_ARN: Reference to created SNS topic

### 3. SNS Topic
- Name: year24082025-notifications
- Email subscription configured
- Lambda publish permissions

### 4. IAM Roles and Policies
- Lambda execution role
- SNS publish permissions
- S3 trigger permissions

## Deployment

### Step 1: Prepare the CloudFormation Template
Save the provided YAML template to a file (e.g., `template.yaml`)

### Step 2: Deploy using AWS CLI
```bash
aws cloudformation create-stack \
  --stack-name s3-notification-system \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_IAM
```

### Step 3: Verify Deployment
1. Check AWS Console for stack creation status
2. Confirm email subscription for SNS notifications
3. Test by uploading a file to the S3 bucket

## Testing the System

1. Upload a file to the S3 bucket:
```bash
aws s3 cp test.txt s3://year24082025/
```

2. Check your email for the notification
3. Verify Lambda function logs in CloudWatch

## Lambda Function Details

The Lambda function is written in Python and performs the following:
1. Captures S3 event details
2. Extracts bucket and file information
3. Formats notification message
4. Publishes to SNS topic

```python
def handler(event, context):
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    message = f"New file uploaded to bucket {bucket}:\nFile name: {key}"
    # Send notification via SNS
    ...
```

## CloudFormation Outputs

The template provides the following outputs:
- BucketName: Name of the created S3 bucket
- SNSTopicARN: ARN of the SNS Topic
- LambdaFunctionARN: ARN of the Lambda function

## Security Considerations

1. IAM roles follow the principle of least privilege
2. S3 bucket versioning enabled for data protection
3. Lambda function runs in a secure VPC configuration
4. SNS topic has appropriate access policies

## Monitoring and Maintenance

- Monitor Lambda function execution in CloudWatch Logs
- Check S3 bucket metrics for upload activities
- Review SNS delivery statistics
- Regularly verify email notification deliveries

## Troubleshooting

1. If notifications are not received:
   - Check SNS subscription confirmation
   - Verify Lambda execution role permissions
   - Review CloudWatch logs for Lambda errors

2. If Lambda isn't triggered:
   - Verify S3 bucket notification configuration
   - Check Lambda permission for S3

## Cost Considerations

This setup uses:
- S3 storage and requests
- Lambda invocations
- SNS message deliveries

Most usage should fall within AWS Free Tier limits for small to medium workloads.

## Limitations

- Email notifications only (can be extended to support other SNS protocols)
- Single region deployment
- Basic file upload notifications (can be enhanced for specific file types)

## Future Enhancements

1. Add support for multiple notification channels
2. Implement file type filtering
3. Add custom notification templates
4. Include file metadata in notifications
5. Add CloudWatch dashboards for monitoring

## Author

**Prasad Bandagale**  
Email: Prasadsb240801@gmail.com

## License

This project is licensed under the MIT License - see the LICENSE file for details.
