# boto3 – Python SDK for Amazon Web Services

[![PyPI version](https://img.shields.io/pypi/v/boto3.svg)](https://pypi.org/project/boto3/)
[![Python versions](https://img.shields.io/pypi/pyversions/boto3.svg)](https://pypi.org/project/boto3/)
[![License](https://img.shields.io/pypi/l/boto3.svg)](LICENSE)
[![Build status](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)](https://github.com/shubhyagami/boto/actions)
[![Docs](https://img.shields.io/badge/docs-AWS%20API%20Reference-blue.svg)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

## 📦 Installation

```bash
pip install boto3
```

## 🚀 Quick start

```python
import boto3

# List all S3 buckets
s3 = boto3.client('s3')
for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

> **Tip** – For multi‑account setups, store named profiles in `~/.aws/credentials` and create a session with `boto3.Session(profile_name="dev")`.

## 💡 Core features

- **Full AWS service coverage** – new APIs are available immediately after release.
- **Dual abstraction** – use low‑level clients (`boto3.client`) or high‑level resources (`boto3.resource`).
- **Automatic retries & pagination** – exponential back‑off is built in.
- **Flexible authentication** – environment variables, credentials file, IAM roles, instance profiles, etc.
- **Debug logging** – `boto3.set_stream_logger('')` prints raw HTTP traffic.
- **Type‑annotated API** – improved IDE support and static type checking.

## 📄 Usage examples

### EC2

```python
import boto3

ec2 = boto3.resource('ec2')
instances = ec2.create_instances(
    ImageId='ami-0abcdef1234567890',
    MinCount=1,
    MaxCount=1,
    InstanceType='t3.micro',
    KeyName='my-key',
)
instance = instances[0]
instance.wait_until_running()
print(f"Instance {instance.id} running at {instance.public_ip_address}")

# … do work …

instance.terminate()
```

### S3

```python
import boto3

# Upload a file
s3 = boto3.client('s3')
s3.upload_file('myfile.txt', 'my-bucket', 'myfile.txt')
```

### DynamoDB

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.create_table(
    TableName='my-table',
    KeySchema=[{'AttributeName': 'id', 'KeyType': 'HASH'}],
    AttributeDefinitions=[{'AttributeName': 'id', 'AttributeType': 'S'}],
    BillingMode='PAY_PER_REQUEST',
)
table.wait_until_exists()
```

## 🔧 Advanced topics

### Client‑side pagination

```python
paginator = s3.get_paginator('list_objects_v2')
for page in paginator.paginate(Bucket='my-bucket'):
    for obj in page.get('Contents', []):
        print(obj['Key'])
```

### Custom retry policy

```python
import botocore
from botocore.config import Config

config = Config(retries={'max_attempts': 10})
client = boto3.client('s3', config=config)
```

### Enable debug logging

```python
import boto3
boto3.set_stream_logger('')
```

## 📚 Changelog

- **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.  
- **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).  
- **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13.

## 🤝 Contributing

1. Fork and clone the repository.  
2. Create a feature branch.  
3. Run `pytest` to confirm all tests pass.  
4. Add or update tests for your changes.  
5. Run `flake8` and `black` to format the code.  
6. Update the changelog with your changes.  
7. Submit a pull request – the CI pipeline must pass before merging.

## 📄 License

Apache 2.0 – see the [LICENSE](LICENSE) file.
