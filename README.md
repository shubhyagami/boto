[K[2m  [2mmodel z-ai/glm-5.3-flash failed, trying next...[0m[0m
[K[2m  [2mmodel deepseek-ai/deepseek-v4.1-flash failed, trying next...[0m[0m
[K[2m  [2mmodel openai/gpt-oss-20b failed, trying next...[0m[0m
[K[2m  [2mmodel openai/gpt-oss-120b failed, trying next...[0m[0m
# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg)
![Python versions](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style: black](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a 100% typed, pure-Python AWS SDK providing complete coverage of the official AWS APIs. Built on top of `botocore`, it enhances the developer experience with full type annotations, automatic retries, simplified pagination, and a more Pythonic interface.

## Installation

```bash
pip install boto
```

## Quick Start

### Basic Usage
`boto` provides two ways to interact with AWS: low-level clients for direct API access and high-level resources for an object-oriented approach.

```python
import boto

# Low-level client: List S3 buckets
s3 = boto.client("s3")
buckets = s3.list_buckets()
print([b["Name"] for b in buckets["Buckets"]])

# High-level resource: Manage an EC2 instance
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
print(f"Instance {instance.id} is running at {instance.public_ip_address}")
instance.terminate()
```

### Using Profiles
To authenticate using a specific shared credentials profile:

```python
session = boto.Session(profile_name="dev")
s3 = session.client("s3")
```

## Core Features

- **Full API Coverage**: Access every available AWS service.
- **Dual Abstractions**: 
    - `boto.client()`: Direct mapping of the AWS API for precise control.
    - `boto.resource()`: High-level, idiomatic Python objects.
- **Type Safety**: 100% type-annotated for superior IDE completion and static analysis (Mypy/Pyright).
- **Robustness**: Built-in retries with exponential backoff and automatic pagination.
- **Flexible Auth**: Native support for environment variables, shared credentials files, IAM roles, and instance profiles.
- **Pure Python**: No compiled extensions required; easy installation across platforms.

## Advanced Usage

### Pagination
Easily handle large result sets using paginators:

```python
s3 = boto.client("s3")
paginator = s3.get_paginator("list_objects_v2")

for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

### Custom Retry Logic
You can customize the retry behavior via the `botocore` config:

```python
from botocore.config import Config

config = Config(retries={"max_attempts": 10})
client = boto.client("s3", config=config)
```

### Debugging
Enable HTTP traffic logging to stdout for troubleshooting:

```python
boto.set_stream_logger("")
```

## Recent Changes

For a detailed history, see [CHANGELOG.md](CHANGELOG.md).

- **1.0.3**: Added S3 Express One-Zone support; fixed SQS visibility-timeout race condition; Python 3.13 test updates.
- **1.0.2**: Optimized DynamoDB batch writes (~15% latency reduction).
- **1.0.1**: Improved EC2 retry logic for throttling events.

## Contributing

1. Fork the repository and create your feature branch.
2. Install dependencies and run the test suite: `pytest`.
3. Ensure all changes are covered by tests.
4. Format code with `black` and lint with `flake8`.
5. Update the `CHANGELOG.md` with your changes.
6. Submit a pull request for review.

## License

Distributed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
