[K[2m  [2mmodel deepseek-ai/deepseek-v4.1-flash failed, trying next...[0m[0m
# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg?label=pypi%20package)
![Python versions](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style: black](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a 100 % typed, pure‑Python AWS SDK that provides complete coverage of the official AWS APIs.  
It is built on top of `botocore`, adds type annotations, automatic retries, pagination, and a convenient Pythonic interface.

## Quick start

```bash
pip install boto
```

```python
import boto

# Low‑level client: list S3 buckets
s3 = boto.client("s3")
print([b["Name"] for b in s3.list_buckets()["Buckets"]])

# High‑level resource: create an EC2 instance
ec2 = boto.resource("ec2")
instances = ec2.create_instances(
    ImageId="ami-0abcdef1234567890",
    MinCount=1,
    MaxCount=1,
    InstanceType="t3.micro",
    KeyName="my-key",
)
instance = instances[0]
instance.wait_until_running()
print(f"Instance {instance.id} running at {instance.public_ip_address}")
instance.terminate()
```

To use a shared credentials profile:

```python
session = boto.Session(profile_name="dev")
s3 = session.client("s3")
```

## Core features

- Full AWS API coverage – every service is available.  
- Dual abstraction:  
  * **Low‑level** clients (`boto.client(...)`) – direct mapping of the AWS API.  
  * **High‑level** resources (`boto.resource(...)`) – Pythonic, idiomatic usage.  
- Built‑in retries with exponential backoff and automatic pagination.  
- Flexible authentication: environment variables, shared credentials, IAM roles, instance profiles, etc.  
- Optional debug logging: `boto.set_stream_logger("")`.  
- 100 % type safety for IDEs and static analysis.  
- Pure Python – no compiled extensions.

## Advanced usage

### Pagination

```python
s3 = boto.client("s3")
for page in s3.get_paginator("list_objects_v2").paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

### Custom retry configuration

```python
from botocore.config import Config
client = boto.client("s3", config=Config(retries={"max_attempts": 10}))
```

### Debug logging

```python
boto.set_stream_logger("")  # logs HTTP traffic to stdout
```

## Changelog

A full changelog is available [here](CHANGELOG.md).

### 1.0.3 (2026‑08‑06)

- Added support for S3 Express One‑Zone.  
- Fixed a race condition in SQS visibility‑timeout handling.  
- Updated tests for Python 3.13.

### 1.0.2 (2026‑07‑25)

- Optimized DynamoDB batch writes (~15 % latency reduction).

### 1.0.1 (2026‑07‑10)

- Improved EC2 retry logic for throttling.

## Contributing

1. Fork and clone the repository.  
2. Create a feature branch.  
3. Run the test suite: `pytest`.  
4. Add or update tests for any changes.  
5. Format the code with `black` and lint with `flake8`.  
6. Update the changelog.  
7. Open a pull request – the CI will run automatically.

## License

Apache 2.0 – see the [LICENSE](LICENSE) file.
