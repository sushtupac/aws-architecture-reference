# AWS Architecture Reference Site

A static reference site documenting common AWS architecture patterns, hosted on Amazon S3 with CloudFront delivery.

## Live Architecture
```
Browser → CloudFront (CDN + HTTPS) → S3 Bucket (private origin)
```

## AWS Services Used

| Service | Role |
|---|---|
| Amazon S3 | Static file hosting, private bucket |
| CloudFront | CDN, HTTPS enforcement, edge caching |
| IAM | Origin Access Control, least-privilege policy |
| AWS CLI | Deployment and bucket configuration |

## Deployment

Files are deployed via AWS CLI sync:
```bash
aws s3 sync . s3://aws-architecture-reference
aws s3 website s3://aws-architecture-reference \
  --index-document index.html \
  --error-document error.html
```

## Project Structure
```
├── index.html      # Main reference site
├── style.css       # Styling
├── error.html      # Custom 404 page
└── README.md       # This file
```

## What I Learned

- How S3 static website hosting works and why CloudFront sits in front of it
- The difference between public bucket access and Origin Access Control (OAC)
- How CloudFront edge locations reduce latency for global users
- How to deploy and manage infrastructure entirely through the AWS CLI
- Why you never expose S3 buckets publicly — CloudFront is the correct entry point

## Local Development

This project uses LocalStack to simulate AWS services locally:
```bash
# Start LocalStack
$env:LOCALSTACK_ACKNOWLEDGE_ACCOUNT_REQUIREMENT=1; localstack start

# Deploy locally
aws --endpoint-url=http://localhost:4566 s3 sync . s3://aws-architecture-reference
```

## Architecture Patterns Documented

- Serverless web application (S3 + CloudFront + API Gateway + Lambda + DynamoDB)
- Data lake pipeline (S3 + Glue + Athena)
- Real-time streaming pipeline (Kinesis + Lambda + DynamoDB)
- ML model deployment (SageMaker + Model Registry + API Gateway)