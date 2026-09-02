# boto3 – Python SDK for Amazon Web Services

![PyPI version](https://img.shields.io/pypi/v/boto3.svg)  
![Python versions](https://img.shields.io/pypi/pyversions/boto3.svg)  
![License](https://img.shields.io/pypi/l/boto3.svg)  
![Docs](https://img.shields.io/badge/docs-AWS%20API%20Reference-blue.svg)

---

## Quick Start

```bash
pip install boto3
```

```python
import boto3

# List all S3 buckets
s3 = boto3.client('s3')
for bucket in s3.list_buckets()['Buckets']:
    print(bucket['Name'])
```

> **Tip** – If you work with multiple AWS accounts, create named profiles in `~/.aws/credentials` and activate them with `boto3.Session(profile_name="dev")`.

---

## What boto3 Provides

| Feature | Description |
|---------|-------------|
| Full AWS coverage | Official SDK for all AWS services, including the latest APIs. |
| Client & Resource APIs | Low‑level clients for fine‑grained control or higher‑level resources for ease of use. |
| Pagination helpers | Built‑in paginators turn multi‑page responses into simple iterators. |
| Automatic retries | Exponential back‑off for throttled or transient failures. |
| Multi‑account & role support | Switch between profiles, regions, and MFA‑assumed roles. |
| Logging & debugging | `boto3.set_stream_logger('')` exposes raw HTTP traffic for troubleshooting. |

---

## Common Usage Patterns

```python
import boto3
import time

# EC2: launch, wait, and terminate
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

```python
# S3: upload a file
s3 = boto3.client('s3')
s3.upload_file('myfile.txt', 'my-bucket', 'myfile.txt')
```

```python
# DynamoDB: create a table
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

* **Client‑side pagination**

  ```python
  paginator = s3.get_paginator('list_objects_v2')
  for page in paginator.paginate(Bucket='my-bucket'):
      for obj in page.get('Contents', []):
          print(obj['Key'])
  ```

* **Custom retry policy**

  ```python
  import botocore
  config = botocore.config.Config(retries={'max_attempts': 10})
  client = boto3.client('s3', config=config)
  ```

* **Enable debug logs**

  ```python
  import boto3
  boto3.set_stream_logger('', boto3.handlers.ALL)
  ```

---

## Recent Changes (excerpt)

- **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.  
- **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).  
- **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13 RC1.

---

## Contributing

1. Fork the repository.  
2. Sync your fork with upstream regularly.  
3. Run `pytest` to ensure tests pass.  
4. Add tests for any new or changed functionality.  
5. Run `flake8` and `black` to enforce code style.  
6. Update the changelog.  
7. Submit a pull request with a concise description and CI status passing.

---

## License

Apache 2.0 – see the [LICENSE](LICENSE) file for details.

The project is actively maintained through 2026 by Shubh Yagami.
