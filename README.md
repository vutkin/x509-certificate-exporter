# About
a prometheus exporter which presents certificate file metrics enabling certificate expiration monitoring

The following metrics are available:
* x509_cert_not_before
* x509_cert_not_after
* x509_cert_expired

# Usage
```
usage: x509-exporter [-h] [-f FILE_PATH] [-d DIR_PATH] [-p PORT] [--debug]

optional arguments:
  -h, --help            show this help message and exit
  -f FILE_PATH, --watch-file FILE_PATH
                        one or more x509 certificate file
  -d DIR_PATH, --watch-dir DIR_PATH
                        one or more directory which contains x509 certificate
                        files
  -p PORT, --port PORT  prometheus exporter listening port
  --debug               enable debug mode
  ```

# Kubernetes
This exporter has been built with a kubernetes usage in mind, so it should be pretty straighforward to setup.

# FAQ
## Why are you using the `not after` timestamp rather than a remaining number of seconds ?
For two reasons.

First, prometheus tends to do better storage consumption when a value stays identical over checks.

Then, it is better to compute the remaining time through a prometheus query as some latency (seconds) can exist between this exporter check and your alert or query being run.

Here is an exemple:
```
x509_cert_not_after - time()
```