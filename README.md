# boto3 – Python SDK for Amazon Web Services

![PyPI version](https://img.shields.io/pypi/v/boto3.svg)
![Python versions](https://img.shields.io/pypi/pyversions/boto3.svg)
![License](https://img.shields.io/pypi/l/boto3.svg)
![Docs](https://img.shields.io/badge/docs-AWS%20API%20Reference-blue.svg)

---

## Overview

boto3 is the official AWS SDK for Python, providing convenient, well‑tested access to nearly all Amazon Web Services. It supports both low‑level **client** interfaces and higher‑level **resource** abstractions, making it suitable for everything from quick scripts to production‑grade applications.

---

## Quickstart

```bash
pip install boto3
```

```python
import boto3

# List all S3 buckets
s3 = boto3.client('s3')
for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

> **Tip** – When working with multiple AWS accounts, store named profiles in `~/.aws/credentials` and activate them with `boto3.Session(profile_name="dev")`.

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Full AWS coverage** | Official SDK for all AWS services, always staying up‑to‑date with the latest APIs. |
| **Client & Resource APIs** | Low‑level clients for fine‑grained control or higher‑level resources for ease of use. |
| **Built‑in pagination** | Paginators turn multi‑page responses into simple iterators. |
| **Automatic retries** | Exponential back‑off for throttled or transient errors. |
| **Multi‑account & role support** | Seamlessly switch profiles, regions, and assumed roles. |
| **Debug logging** | `boto3.set_stream_logger('')` logs raw HTTP traffic for troubleshooting. |

---

## Common Usage Patterns

### EC2

```python
import boto3

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
print(f"Instance {instance.id} running at {instance.public_ip_address}")

# ... do work ...

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
    BillingMode='PAY_PER_REQUEST'
)
table.wait_until_exists()
```

---

## Advanced Topics

### Pagination (client‑side)

```python
paginator = s3.get_paginator('list_objects_v2')
for page in paginator.paginate(Bucket='my-bucket'):
    for obj in page.get('Contents', []):
        print(obj['Key'])
```

### Custom retry policy

```python
import botocore
config = botocore.config.Config(retries={'max_attempts': 10})
client = boto3.client('s3', config=config)
```

### Enable debug logs

```python
import boto3
boto3.set_stream_logger('')
```

---

## Recent Changes (excerpt)

- **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.
- **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).
- **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13.

---

## Contributing

1. Fork the repository.  
2. Sync your fork with the upstream source regularly.  
3. Run `pytest` to confirm all tests pass.  
4. Add tests for any new or altered functionality.  
5. Run `flake8` and `black` to enforce style guidelines.  
6. Update the changelog.  
7. Submit a pull request with a concise description and a passing CI status.

---

## License

Apache 2.0 – see the [LICENSE](LICENSE) file for details.
