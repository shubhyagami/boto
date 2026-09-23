# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg?label=pypi%20package)
![Supported Python](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI status](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a pure-Python client for Amazon Web Services with full type annotations. It exposes all current AWS APIs through low-level clients and high-level resources:

- **Low-level clients** – `boto.client(...)`
- **High-level resources** – `boto.resource(...)`

It works on Python 3.8–3.13, has no C extensions, and is actively maintained.

## Installation

```bash
pip install boto
```

## Requirements

- Python 3.8 or newer
- No compiled extensions

## Getting started

```python
import boto

# Create an S3 client and list buckets
s3 = boto.client("s3")
for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

To use a named profile from `~/.aws/credentials`:

```python
import boto

session = boto.Session(profile_name="dev")
s3 = session.client("s3")
```

## Core features

- **Complete API coverage** – All services are available through the SDK.
- **Dual abstraction** – Use low-level clients or high-level Pythonic resources.
- **Built-in retries and pagination** – Exponential backoff and automatic page iteration.
- **Flexible authentication** – Environment variables, shared credentials, IAM roles, instance profiles, and more.
- **Debug logging** – `boto.set_stream_logger("")` prints raw HTTP traffic.
- **Type-safe** – Type annotations for IDEs and static analysis.
- **Pure Python** – No compiled extensions.

## Usage examples

### EC2

```python
import boto

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
# … do work …
instance.terminate()
```

### S3

```python
import boto

s3 = boto.client("s3")
s3.upload_file("myfile.txt", "my-bucket", "myfile.txt")
```

### DynamoDB

```python
import boto

dynamodb = boto.resource("dynamodb")
table = dynamodb.create_table(
    TableName="my-table",
    KeySchema=[{"AttributeName": "id", "KeyType": "HASH"}],
    AttributeDefinitions=[{"AttributeName": "id", "AttributeType": "S"}],
    BillingMode="PAY_PER_REQUEST",
)
table.wait_until_exists()
```

## Advanced topics

### Paginate a client call

```python
import boto

s3 = boto.client("s3")
paginator = s3.get_paginator("list_objects_v2")
for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

### Custom retry configuration

```python
import boto
from botocore.config import Config

config = Config(retries={"max_attempts": 10})
client = boto.client("s3", config=config)
```

### Enable debug logging for HTTP traffic

```python
import boto

boto.set_stream_logger("")  # logs to stdout
```

## Changelog

### 1.0.3 (2026-08-06)

- Added S3 Express One Zone support.
- Fixed SQS visibility-timeout race.
- Updated tests for Python 3.13.

### 1.0.2 (2026-07-25)

- Optimized DynamoDB batch writes (~15% latency reduction).

### 1.0.1 (2026-07-10)

- Improved EC2 retry logic for throttling.

Full changelog is in [CHANGELOG.md](CHANGELOG.md).

## Contributing

1. Fork the repository and clone it locally.
2. Create a feature branch.
3. Run the test suite: `pytest`.
4. Add or update tests for any code changes.
5. Format the code with `black` and lint with `flake8`.
6. Update the changelog.
7. Open a pull request. CI will run automatically.

## License

Apache 2.0 – see the [LICENSE](LICENSE) file.
