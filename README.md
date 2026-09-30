[K[2m  [2mmodel z-ai/glm-5.3-flash failed, trying next...[0m[0m
[K[2m  [2mmodel deepseek-ai/deepseek-v4.1-flash failed, trying next...[0m[0m
# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg)  
![Python versions](https://img.shields.io/pypi/pyversions/boto.svg)  
![License](https://img.shields.io/pypi/l/boto.svg)  
![CI](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)  
![Code style: black](https://img.shields.io/badge/code_style-black-000000.svg)

## Overview

`boto` is a pure‑Python, fully typed SDK that wraps the official AWS APIs. It builds on `botocore` and adds:

* 100 % type annotations for IDE completion and static analysis.
* Automatic retries with exponential back‑off.
* First‑class paginator support.
* A Pythonic interface for daily use.

No compiled extensions means it installs quickly on any platform.

## Features

| Feature | Description |
|---------|-------------|
| **Full API coverage** | Every service exposed by AWS is available. |
| **Two abstraction levels** | `boto.client()` for low‑level API calls, `boto.resource()` for high‑level, object‑oriented operations. |
| **Type safety** | Safe to use with MyPy, Pyright, or other type checkers. |
| **Built‑in reliability** | Retries, back‑off, and paginator utilities are included. |
| **Flexible authentication** | Supports env vars, shared credentials, IAM roles, and instance profiles. |
| **Pure Python** | No compiled binaries; works on Linux, macOS, Windows. |

## Installation

```bash
pip install boto
```

`boto` respects the standard AWS credential resolution chain:

1. Environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, …)
2. `~/.aws/credentials` file
3. IAM roles or instance profiles (Amazon EC2, ECS, …)

Set a default region with `AWS_DEFAULT_REGION` or pass `region_name` when creating a client/resource.

## Quick Start

```python
import boto

# Low‑level client: list S3 buckets
s3 = boto.client("s3")
buckets = s3.list_buckets()
print([b["Name"] for b in buckets["Buckets"]])

# High‑level resource: launch an EC2 instance
ec2 = boto.resource("ec2")
instances = ec2.create_instances(
    ImageId="ami-0abcdef1234567890",
    MinCount=1,
    MaxCount=1,
    InstanceType="t3.micro",
    KeyName="my-key",
)
instance = instances[0]
print(f"Created instance {instance.id} in {instance.state['Name']} state")
```

## Example: Pagination

```python
s3 = boto.client("s3")
paginator = s3.get_paginator("list_objects_v2")
for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

## Example: Custom Retry Strategy

```python
from boto import Client
from boto.retry import ExponentialBackoff

client = Client(
    "s3",
    retry_strategy=ExponentialBackoff(initial=0.5, max=5, multiplier=2)
)
```

## Changelog

```
2026‑09‑30 – Minor typo fixes and documentation cleanup
2026‑09‑14 – Added paginator helper and example
2026‑08‑20 – Full type‑annotation for V3 APIs
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our development workflow and how to submit pull requests.

## License

`boto` is distributed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
