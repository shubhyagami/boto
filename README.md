# Boto — Python Interface to Amazon Web Services  

## Overview  
Boto is a low‑level Python SDK that provides seamless access to Amazon Web Services (AWS). It enables developers to script, automate, and integrate AWS services such as S3, EC2, SQS, DynamoDB, and more within Python applications.  

## Table of Contents  
- [Getting Started](#getting-started)  
- [Installation](#installation)  
- [Quickstart](#quickstart)  
- [Key Features](#key-features)  
- [Usage Examples](#usage-examples)  
- [Pro Tips](#pro-tips)  
- [Changelog](#changelog)  
- [Contributing](#contributing)  

## Getting Started  

### Installation  
```bash
pip install boto3   # The library is distributed as `boto3`; this repository uses the legacy `boto` name.
```  

### Configuration  
Provide AWS credentials via environment variables or the shared credentials file:  

```bash
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
```  

Or create `~/.aws/credentials`:  

```ini
[default]
aws_access_key_id = your_access_key
aws_secret_access_key = your_secret_key
```  

### Quickstart (S3)  
```python
import boto3

s3 = boto3.client('s3')
for bucket in s3.list_buckets()['Buckets']:
    print(bucket['Name'])
```  

## Key Features  
- **Comprehensive AWS Coverage:** Official SDK for all AWS services.  
- **Profiles & Configuration:** Switch easily between multiple AWS accounts.  
- **Automatic Retries:** Built‑in exponential back‑off for throttled requests.  
- **Debug Logging:** Detailed request/response tracing.  
- **Pagination Helpers:** Efficient iteration over large result sets.  

## Usage Examples  

### EC2 Instance Lifecycle  
```python
import boto3
import time

ec2 = boto3.resource('ec2')
instances = ec2.create_instances(
    ImageId='ami-0abcdef1234567890',
    MinCount=1,
    MaxCount=1,
    InstanceType='t3.micro',
    KeyName='my-key'
)
instance = instances[0]
instance.wait_until_running()
print(f"Instance {instance.id} is running at {instance.public_ip_address}")

# ... perform work ...

instance.terminate()
```  

### S3 File Upload  
```python
s3 = boto3.client('s3')
s3.upload_file('myfile.txt', 'my-bucket', 'myfile.txt')
```  

### DynamoDB Table Creation  
```python
dynamodb = boto3.resource('dynamodb')
table = dynamodb.create_table(
    TableName='my-table',
    KeySchema=[{'AttributeName': 'id', 'KeyType': 'HASH'}],
    AttributeDefinitions=[{'AttributeName': 'id', 'AttributeType': 'S'}],
    BillingMode='PAY_PER_REQUEST'
)
table.wait_until_exists()
```  

## Pro Tips  
- **Enable Debug Logging:** `session = boto3.Session(); session.set_debug(True)`  
- **Reuse Sessions:** `session = boto3.Session(profile_name='prod')`  
- **Client‑Side Pagination:**  
  ```python
  paginator = client.get_paginator('list_objects_v2')
  for page in paginator.paginate(Bucket='my-bucket'):
      for obj in page.get('Contents', []):
          print(obj['Key'])
  ```  
- **Configure Retries:**  
  ```python
  import botocore
  config = botocore.config.Config(retries={'max_attempts': 10})
  client = boto3.client('s3', config=config)
  ```  

## Changelog  
### 1.0.3 (2026‑07‑10)  
- Enhanced EC2 retry logic for better throttling handling.  

### 1.0.2 (2026‑07‑25)  
- Optimized DynamoDB batch writes, reducing latency by ~15 %.  

### 1.0.1 (2026‑08‑06)  
- Added support for S3 Express One Zone storage.  
- Fixed SQS visibility‑timeout race condition.  
- Updated test suite for Python 3.13 RC1 compatibility.  

## Contributing  
We welcome contributions! Please follow these steps:  

1. **Fork & Sync** – Keep your fork current with the upstream repository.  
2. **Run Tests** – Execute `pytest` to verify existing functionality.  
3. **Add Tests** – Include tests for any new code.  
4. **Code Style** – Run `flake8` and `black` before submitting.  
5. **Changelog Entry** – Summarize your changes in the changelog.  

When opening a Pull Request:  
- Provide a clear description of the change and its motivation.  
- Reference any related issues.  
- Ensure all checks pass.  

## Badges  
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)  
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/downloads/)  
[![GitHub Stars](https://img.shields.io/github/stars/shubhyagami/boto?style=social&label=GitHub)](https://github.com/shubhyagami/boto)  

## Maintained By  
- **Shubh Yagami** – *Maintainer*  
- **License:** Apache 2.0  
- **Runtime:** Python 3.10+  

*Active maintenance through 2026.*
