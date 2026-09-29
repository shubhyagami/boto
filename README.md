# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg)
![Python versions](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style: black](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a fully typed, pure-Python SDK for AWS with broad coverage of the official AWS APIs. Built on top of `botocore`, it adds complete type annotations, automatic retries, simplified pagination, and a more Pythonic interface for day-to-day development.

## Features

- **Full API coverage** – access every available AWS service.
- **Two abstraction levels** – `boto.client()` for precise, low-level API control, and `boto.resource()` for idiomatic, high-level Python objects.
- **Type safety** – 100% type-annotated for strong IDE completion and static analysis with Mypy or Pyright.
- **Reliability** – built-in retries with exponential backoff and first-class paginator support.
- **Flexible authentication** – works with environment variables, shared credentials files, IAM roles, and instance profiles.
- **Pure Python** – no compiled extensions, so installation is straightforward on every platform.

## Getting Started

Install the package:

```bash
pip install boto
```

Credentials are resolved through the standard AWS mechanisms: environment variables, the shared credentials file (`~/.aws/credentials`), IAM roles, or instance profiles. Set a default region with `AWS_DEFAULT_REGION` or the `region_name` argument where required.

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
instance = instances[0
