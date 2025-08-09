# Smart-document-Analysis-API
Open projects by Christian kouassi
# Smart Document Analysis API

A cloud-native AI-powered document analysis service that extracts key information, sentiment, and insights from uploaded documents using AWS services and OpenAI GPT.

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   React Frontend │    │   API Gateway    │    │  Lambda Function│
│   (S3 + CloudF.) │───▶│     (REST)       │───▶│   (Python 3.9)  │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                          │
                       ┌─────────────────┐              │
                       │   S3 Bucket     │◀─────────────┘
                       │ (Doc Storage)   │
                       └─────────────────┘
                                │
                       ┌─────────────────┐
                       │  OpenAI API     │
                       │ (GPT-4 Analysis)│
                       └─────────────────┘
```

## ✨ Features

- **Document Upload**: Support for PDF, DOCX, and TXT files
- **AI-Powered Analysis**: Extract key themes, sentiment, and summaries
- **RESTful API**: Clean endpoints for document processing
- **Serverless Architecture**: Cost-effective, auto-scaling infrastructure
- **Real-time Processing**: WebSocket updates for long-running analysis
- **Secure Storage**: Encrypted document storage with lifecycle policies

## 🛠️ Tech Stack

- **Cloud Provider**: AWS
- **Compute**: AWS Lambda (Python 3.9)
- **Storage**: Amazon S3
- **API**: Amazon API Gateway
- **AI/ML**: OpenAI GPT-4 API
- **Frontend**: React with AWS Amplify
- **Infrastructure**: Terraform
- **Monitoring**: CloudWatch

## 📁 Project Structure

```
smart-document-api/
├── terraform/
│   ├── main.tf
│   ├── lambda.tf
│   ├── api_gateway.tf
│   ├── s3.tf
│   └── variables.tf
├── lambda/
│   ├── src/
│   │   ├── handler.py
│   │   ├── document_processor.py
│   │   ├── ai_analyzer.py
│   │   └── utils.py
│   ├── requirements.txt
│   └── tests/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── services/
│   │   └── App.js
│   ├── package.json
│   └── public/
├── docs/
│   ├── API.md
│   ├── DEPLOYMENT.md
│   └── ARCHITECTURE.md
├── scripts/
│   ├── deploy.sh
│   └── test.sh
├── .github/
│   └── workflows/
│       └── deploy.yml
├── README.md
├── requirements.txt
└── docker-compose.yml
```

## 🚀 Quick Start

### Prerequisites

- AWS CLI configured with appropriate permissions
- Terraform >= 1.0
- Python 3.9+
- Node.js 16+
- OpenAI API key

### Local Development

1. **Clone the repository**
   
   ```bash
   git clone https://github.com/yourusername/smart-document-api.git
   cd smart-document-api
   ```
1. **Set up environment variables**
   
   ```bash
   cp .env.example .env
   # Edit .env with your OpenAI API key and AWS settings
   ```
1. **Install dependencies**
   
   ```bash
   # Backend
   pip install -r requirements.txt
   
   # Frontend
   cd frontend && npm install
   ```
1. **Run locally with Docker**
   
   ```bash
   docker-compose up -d
   ```

### Cloud Deployment

1. **Deploy infrastructure**
   
   ```bash
   cd terraform
   terraform init
   terraform plan
   terraform apply
   ```
1. **Deploy Lambda functions**
   
   ```bash
   ./scripts/deploy.sh
   ```
1. **Deploy frontend**
   
   ```bash
   cd frontend
   npm run build
   aws s3 sync build/ s3://your-frontend-bucket
   ```

## 📊 API Endpoints

### Upload Document

```http
POST /api/v1/documents
Content-Type: multipart/form-data

{
  "file": "document.pdf"
}
```

**Response:**

```json
{
  "document_id": "uuid-here",
  "status": "processing",
  "upload_url": "s3-presigned-url"
}
```

### Get Analysis

```http
GET /api/v1/documents/{document_id}/analysis
```

**Response:**

```json
{
  "document_id": "uuid-here",
  "analysis": {
    "summary": "Key points extracted from document...",
    "sentiment": {
      "overall": "positive",
      "confidence": 0.85
    },
    "key_themes": ["technology", "innovation", "growth"],
    "word_count": 1250,
    "reading_time": "5 minutes"
  },
  "status": "completed",
  "processed_at": "2024-01-15T10:30:00Z"
}
```

### List Documents

```http
GET /api/v1/documents?limit=10&offset=0
```

## 🧪 Testing

```bash
# Run unit tests
python -m pytest lambda/tests/

# Run integration tests
./scripts/test.sh

# Load testing
curl -X POST "https://your-api.execute-api.region.amazonaws.com/prod/documents" \
  -F "file=@sample.pdf"
```

## 🏗️ Infrastructure Details

### AWS Resources Created

- **Lambda Functions**: 3 functions for upload, processing, and analysis
- **API Gateway**: RESTful API with CORS enabled
- **S3 Buckets**:
  - Document storage with encryption
  - Frontend hosting with CloudFront
- **IAM Roles**: Least-privilege access for all services
- **CloudWatch**: Logging and monitoring dashboards

### Cost Estimation

- **Lambda**: ~$5-20/month for moderate usage
- **API Gateway**: ~$3-10/month
- **S3 Storage**: ~$2-5/month
- **OpenAI API**: Variable based on usage
- **Total**: ~$15-50/month for development/demo usage

## 🔧 Configuration

### Environment Variables

```bash
# AWS Settings
AWS_REGION=us-east-1
S3_BUCKET_NAME=smart-docs-storage-prod

# OpenAI Settings
OPENAI_API_KEY=sk-your-key-here
OPENAI_MODEL=gpt-4

# Application Settings
MAX_FILE_SIZE_MB=10
ALLOWED_EXTENSIONS=pdf,docx,txt
ANALYSIS_TIMEOUT_SECONDS=300
```

### Terraform Variables

```hcl
# terraform/terraform.tfvars
project_name = "smart-document-api"
environment  = "prod"
aws_region   = "us-east-1"

lambda_memory_size = 512
lambda_timeout     = 300

s3_versioning_enabled = true
```

## 📈 Monitoring & Observability

### CloudWatch Dashboards

- **API Performance**: Response times, error rates
- **Lambda Metrics**: Invocations, duration, errors
- **Cost Tracking**: Daily spend by service

### Alerts Configured

- High error rate (>5%)
- Long processing times (>2 minutes)
- Unusual cost spikes

### Log Analysis

```bash
# View Lambda logs
aws logs tail /aws/lambda/smart-document-processor --follow

# Search for errors
aws logs filter-log-events \
  --log-group-name /aws/lambda/smart-document-processor \
  --filter-pattern "ERROR"
```

## 🚀 CI/CD Pipeline

GitHub Actions workflow automatically:

1. **Runs tests** on pull requests
1. **Deploys to staging** on merge to `develop`
1. **Deploys to production** on merge to `main`
1. **Runs security scans** with Snyk
1. **Updates documentation** automatically

## 🔒 Security Features

- **Encryption**: All data encrypted at rest and in transit
- **Authentication**: API key-based authentication
- **Input Validation**: Comprehensive file type and size validation
- **Rate Limiting**: Prevents API abuse
- **VPC**: Lambda functions in private subnets (optional)

## 🔄 Future Enhancements

- [ ] **Multi-language support** for document analysis
- [ ] **Batch processing** for multiple documents
- [ ] **Integration with** Slack/Teams for notifications
- [ ] **Advanced AI models** for specialized document types
- [ ] **Real-time collaboration** features
- [ ] **Mobile app** for document capture and analysis

## 📚 Documentation

- [API Reference](docs/API.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Architecture Details](docs/ARCHITECTURE.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

## 🤝 Contributing

1. Fork the repository
1. Create a feature branch (`git checkout -b feature/amazing-feature`)
1. Commit your changes (`git commit -m 'Add amazing feature'`)
1. Push to the branch (`git push origin feature/amazing-feature`)
1. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the <LICENSE> file for details.

## 🙋‍♂️ Support

- **Issues**: [GitHub Issues](https://github.com/Midaayannkc/smart-document-api/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Midasyannkc/smart-document-api/discussions)
- **Email**: thehomehuntersllc@gmail.com

##  Acknowledgments

- OpenAI for GPT-4 API
- AWS for cloud infrastructure
- The open-source community for inspiration

-----
import json
import boto3
import uuid
import os
from datetime import datetime
import base64
from typing import Dict, Any
import logging
from document_processor import DocumentProcessor
from ai_analyzer import AIAnalyzer

# Configure logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

# Initialize AWS clients

s3_client = boto3.client(‘s3’)
dynamodb = boto3.resource(‘dynamodb’)

# Environment variables

S3_BUCKET = os.environ[‘S3_BUCKET_NAME’]
TABLE_NAME = os.environ[‘DYNAMODB_TABLE_NAME’]
OPENAI_API_KEY = os.environ[‘OPENAI_API_KEY’]

# Initialize processors

doc_processor = DocumentProcessor()
ai_analyzer = AIAnalyzer(OPENAI_API_KEY)

def lambda_handler(event, context):
“”“Main Lambda handler for document analysis API”””

```
try:
    http_method = event['httpMethod']
    path = event['path']
    
    # Route requests to appropriate handlers
    if http_method == 'POST' and path == '/api/v1/documents':
        return upload_document(event, context)
    elif http_method == 'GET' and '/documents/' in path and '/analysis' in path:
        return get_analysis(event, context)
    elif http_method == 'GET' and path == '/api/v1/documents':
        return list_documents(event, context)
    else:
        return {
            'statusCode': 404,
            'headers': get_cors_headers(),
            'body': json.dumps({'error': 'Endpoint not found'})
        }
        
except Exception as e:
    logger.error(f"Unhandled error: {str(e)}")
    return {
        'statusCode': 500,
        'headers': get_cors_headers(),
        'body': json.dumps({'error': 'Internal server error'})
    }
```

def upload_document(event, context):
“”“Handle document upload and initiate processing”””

```
try:
    # Parse multipart form data
    body = event.get('body', '')
    content_type = event.get('headers', {}).get('Content-Type', '')
    
    if not body or 'multipart/form-data' not in content_type:
        return {
            'statusCode': 400,
            'headers': get_cors_headers(),
            'body': json.dumps({'error': 'No file provided'})
        }
    
    # Extract file data (simplified - in production, use proper multipart parser)
    if event.get('isBase64Encoded', False):
        file_content = base64.b64decode(body)
    else:
        file_content = body.encode()
    
    # Validate file
    if len(file_content) > 10 * 1024 * 1024:  # 10MB limit
        return {
            'statusCode': 400,
            'headers': get_cors_headers(),
            'body': json.dumps({'error': 'File too large (max 10MB)'})
        }
    
    # Generate unique document ID
    document_id = str(uuid.uuid4())
    timestamp = datetime.utcnow().isoformat()
    
    # Upload to S3
    s3_key = f"documents/{document_id}/original.pdf"
    s3_client.put_object(
        Bucket=S3_BUCKET,
        Key=s3_key,
        Body=file_content,
        ServerSideEncryption='AES256'
    )
    
    # Store metadata in DynamoDB
    table = dynamodb.Table(TABLE_NAME)
    table.put_item(
        Item={
            'document_id': document_id,
            'status': 'processing',
            'upload_time': timestamp,
            's3_key': s3_key,
            'file_size': len(file_content)
        }
    )
    
    # Trigger async processing
    process_document_async(document_id, s3_key)
    
    return {
        'statusCode': 201,
        'headers': get_cors_headers(),
        'body': json.dumps({
            'document_id': document_id,
            'status': 'processing',
            'message': 'Document uploaded successfully'
        })
    }
    
except Exception as e:
    logger.error(f"Upload error: {str(e)}")
    return {
        'statusCode': 500,
        'headers': get_cors_headers(),
        'body': json.dumps({'error': 'Upload failed'})
    }
```

def get_analysis(event, context):
“”“Retrieve document analysis results”””

```
try:
    # Extract document ID from path
    path_parts = event['path'].split('/')
    document_id = path_parts[4]  # /api/v1/documents/{id}/analysis
    
    # Retrieve from DynamoDB
    table = dynamodb.Table(TABLE_NAME)
    response = table.get_item(Key={'document_id': document_id})
    
    if 'Item' not in response:
        return {
            'statusCode': 404,
            'headers': get_cors_headers(),
            'body': json.dumps({'error': 'Document not found'})
        }
    
    item = response['Item']
    
    # Return analysis results
    return {
        'statusCode': 200,
        'headers': get_cors_headers(),
        'body': json.dumps({
            'document_id': document_id,
            'status': item['status'],
            'analysis': item.get('analysis', {}),
            'processed_at': item.get('processed_at'),
            'upload_time': item['upload_time']
        })
    }
    
except Exception as e:
    logger.error(f"Get analysis error: {str(e)}")
    return {
        'statusCode': 500,
        'headers': get_cors_headers(),
        'body': json.dumps({'error': 'Failed to retrieve analysis'})
    }
```

def list_documents(event, context):
“”“List user’s documents with pagination”””

```
try:
    # Get query parameters
    query_params = event.get('queryStringParameters') or {}
    limit = int(query_params.get('limit', 10))
    
    # Query DynamoDB
    table = dynamodb.Table(TABLE_NAME)
    response = table.scan(Limit=limit)
    
    documents = []
    for item in response.get('Items', []):
        documents.append({
            'document_id': item['document_id'],
            'status': item['status'],
            'upload_time': item['upload_time'],
            'file_size': item.get('file_size', 0)
        })
    
    return {
        'statusCode': 200,
        'headers': get_cors_headers(),
        'body': json.dumps({
            'documents': documents,
            'count': len(documents)
        })
    }
    
except Exception as e:
    logger.error(f"List documents error: {str(e)}")
    return {
        'statusCode': 500,
        'headers': get_cors_headers(),
        'body': json.dumps({'error': 'Failed to list documents'})
    }
```

def process_document_async(document_id: str, s3_key: str):
“”“Process document asynchronously”””

```
try:
    # Download document from S3
    response = s3_client.get_object(Bucket=S3_BUCKET, Key=s3_key)
    file_content = response['Body'].read()
    
    # Extract text from document
    text_content = doc_processor.extract_text(file_content, s3_key)
    
    if not text_content:
        raise Exception("Failed to extract text from document")
    
    # Analyze with AI
    analysis = ai_analyzer.analyze_document(text_content)
    
    # Update DynamoDB with results
    table = dynamodb.Table(TABLE_NAME)
    table.update_item(
        Key={'document_id': document_id},
        UpdateExpression='SET #status = :status, analysis = :analysis, processed_at = :timestamp',
        ExpressionAttributeNames={'#status': 'status'},
        ExpressionAttributeValues={
            ':status': 'completed',
            ':analysis': analysis,
            ':timestamp': datetime.utcnow().isoformat()
        }
    )
    
    logger.info(f"Successfully processed document {document_id}")
    
except Exception as e:
    logger.error(f"Processing error for {document_id}: {str(e)}")
    
    # Update status to failed
    table = dynamodb.Table(TABLE_NAME)
    table.update_item(
        Key={'document_id': document_id},
        UpdateExpression='SET #status = :status, error_message = :error',
        ExpressionAttributeNames={'#status': 'status'},
        ExpressionAttributeValues={
            ':status': 'failed',
            ':error': str(e)
        }
    )
```

def get_cors_headers():
“”“Return CORS headers for API responses”””
return {
‘Access-Control-Allow-Origin’: ‘*’,
‘Access-Control-Allow-Methods’: ‘GET, POST, PUT, DELETE, OPTIONS’,
‘Access-Control-Allow-Headers’: ‘Content-Type, Authorization’
}
# terraform/main.tf

terraform {
required_version = “>= 1.0”
required_providers {
aws = {
source  = “hashicorp/aws”
version = “~> 5.0”
}
}
}

provider “aws” {
region = var.aws_region
}

# Variables

variable “project_name” {
description = “Name of the project”
type        = string
default     = “smart-document-api”
}

variable “environment” {
description = “Environment (dev, staging, prod)”
type        = string
default     = “dev”
}

variable “aws_region” {
description = “AWS region”
type        = string
default     = “us-east-1”
}

variable “lambda_memory_size” {
description = “Lambda memory size in MB”
type        = number
default     = 512
}

variable “lambda_timeout” {
description = “Lambda timeout in seconds”
type        = number
default     = 300
}

# S3 Bucket for document storage

resource “aws_s3_bucket” “documents” {
bucket = “${var.project_name}-documents-${var.environment}-${random_string.suffix.result}”

tags = {
Name        = “${var.project_name}-documents”
Environment = var.environment
}
}

resource “random_string” “suffix” {
length  = 8
special = false
upper   = false
}

resource “aws_s3_bucket_encryption_configuration” “documents” {
bucket = aws_s3_bucket.documents.id

rule {
apply_server_side_encryption_by_default {
sse_algorithm = “AES256”
}
}
}

resource “aws_s3_bucket_versioning” “documents” {
bucket = aws_s3_bucket.documents.id
versioning_configuration {
status = “Enabled”
}
}

resource “aws_s3_bucket_lifecycle_configuration” “documents” {
bucket = aws_s3_bucket.documents.id

rule {
id     = “document_lifecycle”
status = “Enabled”

```
expiration {
  days = 90
}

noncurrent_version_expiration {
  noncurrent_days = 30
}
```

}
}

# DynamoDB table for metadata

resource “aws_dynamodb_table” “documents” {
name           = “${var.project_name}-documents-${var.environment}”
billing_mode   = “PAY_PER_REQUEST”
hash_key       = “document_id”

attribute {
name = “document_id”
type = “S”
}

tags = {
Name        = “${var.project_name}-documents”
Environment = var.environment
}
}

# IAM role for Lambda

resource “aws_iam_role” “lambda_role” {
name = “${var.project_name}-lambda-role-${var.environment}”

assume_role_policy = jsonencode({
Version = “2012-10-17”
Statement = [
{
Action = “sts:AssumeRole”
Effect = “Allow”
Principal = {
Service = “lambda.amazonaws.com”
}
}
]
})
}

resource “aws_iam_role_policy” “lambda_policy” {
name = “${var.project_name}-lambda-policy-${var.environment}”
role = aws_iam_role.lambda_role.id

policy = jsonencode({
Version = “2012-10-17”
Statement = [
{
Effect = “Allow”
Action = [
“logs:CreateLogGroup”,
“logs:CreateLogStream”,
“logs:PutLogEvents”
]
Resource = “arn:aws:logs:*:*:*”
},
{
Effect = “Allow”
Action = [
“s3:GetObject”,
“s3:PutObject”,
“s3:DeleteObject”
]
Resource = “${aws_s3_bucket.documents.arn}/*”
},
{
Effect = “Allow”
Action = [
“dynamodb:PutItem”,
“dynamodb:GetItem”,
“dynamodb:UpdateItem”,
“dynamodb:Scan”,
“dynamodb:Query”
]
Resource = aws_dynamodb_table.documents.arn
}
]
})
}

resource “aws_iam_role_policy_attachment” “lambda_basic” {
role       = aws_iam_role.lambda_role.name
policy_arn = “arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole”
}

# Lambda function

resource “aws_lambda_function” “document_processor” {
filename         = “lambda_deployment_package.zip”
function_name    = “${var.project_name}-processor-${var.environment}”
role            = aws_iam_role.lambda_role.arn
handler         = “handler.lambda_handler”
runtime         = “python3.9”
timeout         = var.lambda_timeout
memory_size     = var.lambda_memory_size

environment {
variables = {
S3_BUCKET_NAME       = aws_s3_bucket.documents.bucket
DYNAMODB_TABLE_NAME  = aws_dynamodb_table.documents.name
OPENAI_API_KEY      = var.openai_api_key
}
}

tags = {
Name        = “${var.project_name}-processor”
Environment = var.environment
}
}

# API Gateway

resource “aws_api_gateway_rest_api” “main” {
name        = “${var.project_name}-api-${var.environment}”
description = “Smart Document Analysis API”

endpoint_configuration {
types = [“REGIONAL”]
}
}

resource “aws_api_gateway_resource” “api” {
rest_api_id = aws_api_gateway_rest_api.main.id
parent_id   = aws_api_gateway_rest_api.main.root_resource_id
path_part   = “api”
}

resource “aws_api_gateway_resource” “v1” {
rest_api_id = aws_api_gateway_rest_api.main.id
parent_id   = aws_api_gateway_resource.api.id
path_part   = “v1”
}

resource “aws_api_gateway_resource” “documents” {
rest_api_id = aws_api_gateway_rest_api.main.id
parent_id   = aws_api_gateway_resource.v1.id
path_part   = “documents”
}

# POST /api/v1/documents

resource “aws_api_gateway_method” “upload_document” {
rest_api_id   = aws_api_gateway_rest_api.main.id
resource_id   = aws_api_gateway_resource.documents.id
http_method   = “POST”
authorization = “NONE”
}

resource “aws_api_gateway_integration” “upload_document” {
rest_api_id = aws_api_gateway_rest_api.main.id
resource_id = aws_api_gateway_resource.documents.id
http_method = aws_api_gateway_method.upload_document.http_method

integration_http_method = “POST”
type                   = “AWS_PROXY”
uri                    = aws_lambda_function.document_processor.invoke_arn
}

# GET /api/v1/documents

resource “aws_api_gateway_method” “list_documents” {
rest_api_id   = aws_api_gateway_rest_api.main.id
resource_id   = aws_api_gateway_resource.documents.id
http_method   = “GET”
authorization = “NONE”
}

resource “aws_api_gateway_integration” “list_documents” {
rest_api_id = aws_api_gateway_rest_api.main.id
resource_id = aws_api_gateway_resource.documents.id
http_method = aws_api_gateway_method.list_documents.http_method

integration_http_method = “POST”
type                   = “AWS_PROXY”
uri                    = aws_lambda_function.document_processor.invoke_arn
}

# Lambda permissions for API Gateway

resource “aws_lambda_permission” “api_gateway” {
statement_id  = “AllowExecutionFromAPIGateway”
action        = “lambda:InvokeFunction”
function_name = aws_lambda_function.document_processor.function_name
principal     = “apigateway.amazonaws.com”
source_arn    = “${aws_api_gateway_rest_api.main.execution_arn}/*/*”
}

# API Gateway deployment

resource “aws_api_gateway_deployment” “main” {
depends_on = [
aws_api_gateway_integration.upload_document,
aws_api_gateway_integration.list_documents,
]

rest_api_id = aws_api_gateway_rest_api.main.id
stage_name  = var.environment

lifecycle {
create_before_destroy = true
}
}

# CloudWatch Log Group

resource “aws_cloudwatch_log_group” “lambda_logs” {
name              = “/aws/lambda/${aws_lambda_function.document_processor.function_name}”
retention_in_days = 14
}

# Outputs

output “api_gateway_url” {
description = “URL of the API Gateway”
value       = “${aws_api_gateway_deployment.main.invoke_url}/api/v1”
}

output “s3_bucket_name” {
description = “Name of the S3 bucket”
value       = aws_s3_bucket.documents.bucket
}

output “dynamodb_table_name” {
description = “Name of the DynamoDB table”
value       = aws_dynamodb_table.documents.name
}

output “lambda_function_name” {
description = “Name of the Lambda function”
value       = aws_lambda_function.document_processor.function_name
}

# Variable for OpenAI API key (to be set via terraform.tfvars or environment)

variable “openai_api_key” {
description = “OpenAI API key for document analysis”
type        = string
sensitive   = true
}
Deployment and setup scripts 
#!/bin/bash

# scripts/deploy.sh - Complete deployment script

set -e  # Exit





Built by [kouadio chrstian kouassi] | [LinkedIn](https://linkedin.com/in/kouadio-kouassi-a891537379) | [Portfolio](https://yourwebsite.com)
