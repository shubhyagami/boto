# boto – Python SDK for Amazon Web Services

[![PyPI version](https://img.shields.io/pypi/v/boto3.svg)](https://pypi.org/project/boto3/)
[![Python versions](https://img.shields.io/pypi/pyversions/boto3.svg)](https://pypi.org/project/boto3/)

[![License](https://img.shields.io/pypi/l/boto3.svg)](LICENSE)
[![Build status](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)](https://github.com/shubhyagami/boto/actions)
[![Docs](https://img.shields.io/badge/docs-AWS%20API%20Reference-blue.svg)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

---

## Quick start

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

> **Tip** – For multi‑account setups store named profiles in `~/.aws/credentials` and use
> `boto3.Session(profile_name="dev")`.

---

## What’s boto3?

boto3 is the official AWS SDK for Python. It gives you:

* **Low‑level clients** for fine‑grained control over any AWS service.
* **High‑level resources** (e.g., `boto3.resource('s3')`) that wrap the client API in a more Pythonic, object‑oriented style.
* Automatic handling of retries, pagination, and region selection.
* Seamless integration with AWS credentials, roles, and profiles.

---

## Key features

| Feature | Description |
|---------|--------------|
| Full AWS coverage | Official SDK for every AWS service, always updated with the latest APIs. |
| Client & Resource APIs | Choose the right abstraction for your use case. |
| Paginators | Turn multi‑page responses into iterators with `get_paginator`. |
| Automatic retries | Exponential back‑off on throttling or transient failures. |
| Multi‑account/role support | Switch profiles, regions, and assume roles effortlessly. |
| Debug logging | `boto3.set_stream_logger('')` logs raw HTTP traffic. |

---

## Common usage patterns

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

## Advanced topics

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

## Changelog – recent changes

- **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.
- **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).
- **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13.

---

## Contributing

1. Fork the repo and clone your fork.  
2. Make your changes in a feature branch.  
3. Run `pytest` to ensure all tests pass.  
4. Add or update tests for any new or modified functionality.  
5. Run `flake8` and `black` to format the code.  
6. Update the changelog with your changes.  
7. Submit a pull request; ensure the CI pipeline passes.

---

## License

Apache 2.0 – see the [LICENSE](LICENSE) file for details.
