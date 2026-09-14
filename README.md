# boto3 – Python SDK for Amazon Web Services

![PyPI version](https://img.shields.io/pypi/v/boto3.svg)
![Supported Python](https://img.shields.io/pypi/pyversions/boto3.svg)
![License](https://img.shields.io/pypi/l/boto3.svg)
![Build status](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)

**boto3** is Amazon Web Services’ official Python library. It provides a thin, well‑documented interface to every AWS service, all in a single, actively maintained package.

---

## Getting started

```bash
pip install boto3
```

```python
import boto3

# List S3 buckets
s3 = boto3.client('s3')
for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

> For multi‑account workflows, store named profiles in `~/.aws/credentials` and create a session with `boto3.Session(profile_name="dev")`.

---

## Core features

| Feature | Description |
|---------|-------------|
| **Complete service coverage** | Access all current AWS APIs immediately after release. |
| **Dual abstraction** | Work with low‑level clients (`boto3.client`) or high‑level resources (`boto3.resource`). |
| **Automatic retries & pagination** | Exponential back‑off and built‑in paginators save you from re‑implementing error handling. |
| **Flexible authentication** | Supports env vars, credentials files, IAM roles, instance profiles, and more. |
| **Debug logging** | `boto3.set_stream_logger('')` prints raw HTTP traffic for troubleshooting. |
| **Type annotated** | Pydantic‑style annotations aid IDEs and static type checkers. |

---

## Usage examples

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

---

## Advanced topics

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

---

## Changelog

* **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.  
* **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).  
* **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13.

*(Full changelog is in [CHANGELOG.md](CHANGELOG.md))*


---

## Contributing

1. Fork the repo and clone it locally.  
2. Create a feature branch.  
3. Run the test suite: `pytest`.  
4. Add or update tests for any changes.  
5. Format the code with `black` and lint with `flake8`.  
6. Update the changelog before submitting a pull request.  
7. The CI pipeline must pass before the PR is merged.

---

## License

Apache 2.0 – see the [LICENSE](LICENSE) file.
