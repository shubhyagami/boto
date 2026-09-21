# boto – Fully typed Python SDK for AWS

![PyPI version](https://img.shields.io/pypi/v/boto.svg?label=pypi%20package)
![Supported Python](https://img.shields.io/pypi/pyversions/boto.svg)
![License](https://img.shields.io/pypi/l/boto.svg)
![CI status](https://github.com/shubhyagami/boto/actions/workflows/python.yml/badge.svg)
![Code style](https://img.shields.io/badge/code_style-black-000000.svg)

`boto` is a type‑annotated Python client for Amazon Web Services.  
It exposes every current AWS API through both low‑level clients (`boto.client`) and high‑level resources (`boto.resource`).  
The library works on all supported Python 3.x versions (3.8–3.13) and is fully maintained.

---

## Installation

```bash
pip install boto
```

---

## Quick start

```python
import boto

# List all S3 buckets
s3 = boto.client("s3")
for bucket in s3.list_buckets()["Buckets"]:
    print(bucket["Name"])
```

For multi‑account setups store profiles in `~/.aws/credentials` and start a session with:

```python
session = boto.Session(profile_name="dev")
```

---

## Core features

- **All APIs** – Immediate access to every AWS service.
- **Dual abstraction** – Low‑level AWS clients and high‑level Pythonic resources.
- **Automatic retries & built‑in pagination** – Exponential back‑off, safe defaults.
- **Flexible authentication** – Environment variables, credentials files, IAM roles, instance profiles, etc.
- **Debug logging** – `boto.set_stream_logger("")` prints raw HTTP traffic.
- **Type safety** – Pydantic‑style annotations for IDEs and static analysis.
- **Zero runtime dependencies** – Pure Python, no C extensions.

---

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

---

## Advanced topics

### Paginate a client call

```python
paginator = s3.get_paginator("list_objects_v2")
for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

### Custom retry configuration

```python
import botocore
from botocore.config import Config

config = Config(retries={"max_attempts": 10})
client = boto.client("s3", config=config)
```

### Enable debug logging for HTTP traffic

```python
import boto
boto.set_stream_logger("")  # logs to stdout
```

---

## Changelog (excerpt)

- **1.0.3 (2026‑07‑10)** – Improved EC2 retry logic for throttling.
- **1.0.2 (2026‑07‑25)** – Optimized DynamoDB batch writes (~15 % latency reduction).
- **1.0.1 (2026‑08‑06)** – Added S3 Express One Zone support, fixed SQS visibility‑timeout race, updated tests for Python 3.13.

*(Full changelog is in [CHANGELOG.md](CHANGELOG.md))*  

---

## Contributing

1. Fork and clone the repository.  
2. Create a feature branch.  
3. Run tests: `pytest`.  
4. Add or update tests for any changes.  
5. Format code with `black`, lint with `flake8`.  
6. Update the change log.  
7. Submit a pull request – the CI pipeline will run automatically.

---

## License

Apache 2.0 – see the [LICENSE](LICENSE) file.
