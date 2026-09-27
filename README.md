[K[2m  [2mmodel z-ai/glm-5.3-flash failed, trying next...[0m[0m
# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg)
![Python versions](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style: black](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a fully typed, pure-Python AWS SDK with broad coverage of the official AWS APIs. Built on top of `botocore`, it adds full type annotations, automatic retries, simplified pagination, and a more Pythonic interface for day-to-day development.

## Getting Started

Install the package:

```bash
pip install boto
```

Configure credentials using any standard AWS mechanism: environment variables, a shared credentials file (`~/.aws/credentials`), IAM roles, or instance profiles. Set a default region when needed.

## Quick Start

### Basic usage

`boto` exposes two main interfaces:

- `boto.client()`: low-level clients that map directly to AWS APIs.
- `boto.resource()`: high-level, object-oriented resources.

```python
import boto

# Low-level client: list S3 buckets
s3 = boto.client("s3")
buckets = s3.list_buckets()
print([b["Name"] for b in buckets["Buckets"]])

# High-level resource: launch and manage an EC2 instance
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

### Using profiles

```python
session = boto.Session(profile_name="dev")
s3 = session.client("s3")
```

## Features

- **Full API coverage**: access every available AWS service.
- **Two abstraction levels**:
  - `boto.client()` for precise, low-level API control.
  - `boto.resource()` for idiomatic, high-level Python objects.
- **Type safety**: 100% type-annotated for strong IDE completion and static analysis with Mypy or Pyright.
- **Reliability**: built-in retries with exponential backoff and paginator support.
- **Flexible authentication**: supports environment variables, shared credentials files, IAM roles, and instance profiles.
- **Pure Python**: no compiled extensions, making installation straightforward across platforms.

## Advanced Usage

### Pagination

```python
s3 = boto.client("s3")
paginator = s3.get_paginator("list_objects_v2")

for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

### Custom retry logic

```python
from botocore.config import Config

config = Config(retries={"max_attempts": 10})
client = boto.client("s3", config=config)
```

### Debugging

Enable HTTP traffic logging for troubleshooting:

```python
boto.set_stream_logger("")
```

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for the full history.

- **1.0.3**: Added S3 Express One-Zone support; fixed an SQS visibility-timeout race condition; updated tests for Python 3.13.
- **1.0.2**: Optimized DynamoDB batch writes, reducing latency by roughly 15%.
- **1.0.1**: Improved EC2 retry behavior for throttling events.

## Contributing

1. Fork the repository and create a feature branch.
2. Install dependencies and run the test suite with `pytest`.
3. Add or update tests for your changes.
4. Format code with `black` and lint with `flake8`.
5. Update `CHANGELOG.md`.
6. Submit a pull request for review.

## License

Distributed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
