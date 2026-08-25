# Boto — Python Interface to Amazon Web Services

### Overview

Boto is a Python package providing interfaces to Amazon Web Services (AWS). It empowers developers to create scripts and applications interacting with AWS services like S3, EC2, SQS, and more.

### Table of Contents

- [Getting Started](#getting-started)
- [Key Features](#key-features)
- [Example: Automating EC2 Instance Lifecycle](#example-automating-ec2-instance-lifecycle)
- [Pro Tips](#pro-tips)
- [Changelog](#changelog)
- [Contributing](#contributing)

### Getting Started

#### Install Boto

To get started, install Boto using pip:

```bash
pip install boto
```

#### Configure Boto

To use Boto, you'll need to configure it with your AWS credentials. You can do this by setting environment variables or by storing them in `~/.aws/credentials`:

```bash
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
```

#### Use Boto

Here's an example of how to use Boto to connect to S3:

```python
import boto
import time

s3 = boto.connect_s3()
for bucket in s3.get_all_buckets():
    print(bucket.name)
```

### Key Features

- **Universal AWS Support:** Boto provides interfaces to multiple AWS services, including compute, storage, databases, and messaging.
- **Flexible Configuration:** Easily switch between multiple AWS accounts using profiles.
- **Robust Retry Mechanism:** Built-in exponential backoff and retries to handle API throttling gracefully.
- **Streamlined Debugging:** Built-in debug logging to easily trace API calls and troubleshoot issues.

### Example: Automating EC2 Instance Lifecycle

Need to spin up a fleet of EC2 instances for a batch job and clean them up automatically? Boto makes it trivial:

```python
import boto
import time

ec2 = boto.connect_ec2()

# Launch an instance
reservation = ec2.run_instances('ami-0abcdef1234567890',
                               key_name='my-key',
                               instance_type='t3.micro',
                               min_count=1, max_count=1)
instance = reservation.instances[0]
print(f"Launching {instance.id}...")

# Wait for it to be running
while instance.state != 'running':
    time.sleep(5)
    instance.update()

print(f"{instance.id} is now running at {instance.ip_address}")

# ... do your work ...

# Terminate when done
ec2.terminate_instances(instance_ids=[instance.id])
print(f"{instance.id} terminated.")
```

### Pro Tips

- **Efficient Pagination:** Use `boto.s3.bucketlistresultset` to paginate through large result sets without running into memory issues.
- **Retries:** Enable automatic retries by setting `boto.config.set('Boto', 'num_retries', 3)`.
- **Profiles:** Switch between multiple AWS accounts with `boto.config.set('Boto', 'profile', 'myprofile')`.
- **Logging:** Turn on debug logging to see API calls: `boto.set_stream_logger('boto')`.

### Changelog

### Version 1.0.0 (2026-08-25)

- **Initial Release:** Boto is now available for public use.

### Version 1.0.1 (2026-08-06)

- **New Feature:** Added support for Amazon S3 Express One Zone storage class.
- **Bug Fix:** Fixed a race condition in `boto.sqs` message visibility timeout handling.
- **Internal Upgrade:** Upgraded test suite to Python 3.13 compatibility (RC1).

### Version 1.0.2 (2026-07-25)

- **Improvement:** Optimized DynamoDB batch writes to reduce latency by 15%.

### Version 1.0.3 (2026-07-10)

- **Enhancement:** Improved retry logic in `boto.ec2` to better handle throttling.

### Contributing

Contributions are welcome! Please read the guidelines below before opening a pull request.

1. **Pull Requests:** Provide a clear description of your changes. Explain not just what you changed, but why.
2. **Testing:** Ensure all tests pass via `pytest`. Add tests for any new functionality to maintain code coverage.
3. **Code Review:** At least one maintainer must approve your branch before it can be merged.

**Before You Open a PR:**
- [ ] Ensure your fork is synchronized with the central repository.
- [ ] Run `flake8` and `black` to ensure formatting complies with project standards.
- [ ] Update the Changelog with a summary of your changes.
- [ ] Add test cases for any new features or bug fixes.

---

*Maintained by Shubh Yagami.*
*Built with Python 3.10 and above.*
*License: Apache 2.0.*
*Maintenance active through 2026.*
*GitHub stars: [Shubh Yagami](https://github.com/shubhyagami/boto)*

### Features

* Supports multiple AWS services, including compute, storage, databases, and messaging
* Flexible configuration with profiles
* Robust retry mechanism to handle API throttling
* Streamlined debugging with built-in logging

### What's New

* Version 1.0.1 includes support for Amazon S3 Express One Zone storage class, fixed a race condition in `boto.sqs` message visibility timeout handling, and upgraded the test suite to Python 3.13 compatibility (RC1).
* Version 1.0.2 includes optimization of DynamoDB batch writes to reduce latency by 15%.
* Version 1.0.3 includes improved retry logic in `boto.ec2` to better handle throttling.

### Installation

* Install Boto using pip: `pip install boto`

### Configuration

* Configure Boto using environment variables or `~/.aws/credentials`: `export AWS_ACCESS_KEY_ID=your_access_key; export AWS_SECRET_ACCESS_KEY=your_secret_key`

### Usage

* Use Boto to connect to AWS services: `s3 = boto.connect_s3()`
* Use Boto to automate EC2 instance lifecycle: `ec2 = boto.connect_ec2(); reservation = ec2.run_instances('ami-0abcdef1234567890', key_name='my-key', instance_type='t3.micro', min_count=1, max_count=1)`
