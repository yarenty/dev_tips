---
title: "MinIO — high-performance S3-compatible object storage"
main_link: https://min.io/
keywords: [minio, s3, object-storage, self-hosted, distributed-storage, erasure-coding, golang]
status: reviewed
---

# MinIO — high-performance S3-compatible object storage

**Main link:** <https://min.io/>

Docs: <https://min.io/docs/minio/linux/index.html> · Repo: <https://github.com/minio/minio>

> **Note on filing**: this article lives under `programming/unix_systems/` for historical reasons. MinIO is **not** a Unix system — it's a storage server that happens to run on Unix-like operating systems. The misclassification is preserved here rather than reorganized.

## Summary

MinIO is an open-source, self-hostable, S3-API-compatible object storage server written in Go. A single binary runs on Linux/macOS/Windows; the same binary can serve a single drive on a laptop or a multi-petabyte distributed cluster across racks of servers. Erasure coding (Reed-Solomon by default) provides drive-level redundancy without requiring an underlying RAID. Because it implements the AWS S3 API at the wire level, every existing S3 client library — `aws-sdk-go`, `boto3`, `aws-sdk-js`, `aws-sdk-java`, the AWS CLI, `s3cmd`, `rclone`, anything supporting `s3://` URIs — works against it unchanged.

## Insight

Where MinIO fits well:

- **Self-hosted S3** for on-prem, edge, air-gapped, dev/test, or "we can't egress to AWS" environments.
- **Drop-in test backend.** `minio server /tmp/data` gives you a working S3 endpoint in seconds for integration tests without mocking.
- **Multi-tenant private cloud** — IAM, bucket policies, server-side encryption, object lock all map to S3 semantics.
- **Lakehouse / analytics storage layer.** MinIO is one of the standard backends for Parquet-on-object-storage stacks (Trino, Spark, DuckDB, Iceberg, Delta Lake).
- **Distributed deployment with erasure coding** — a 4+4 or 8+4 erasure-coded cluster survives drive failures without RAID complexity.

License caveats (read these before deploying):

- The community edition is **AGPLv3**. If you embed MinIO in a hosted service or modify the source, the AGPL's network-use clause requires you to publish your modifications. For most internal deployments this is fine; for SaaS it can be a real concern.
- The "Enterprise" edition is a separately-licensed commercial offering.
- MinIO has, over time, removed several previously-open features from the community edition (the web console UI in particular has been dramatically reduced). This has been controversial; alternatives like [Garage](https://garagehq.deuxfleurs.fr/), [SeaweedFS](https://seaweedfs.com/), and the AWS-compatible mode of [Ceph (RGW)](https://docs.ceph.com/en/latest/radosgw/) are worth evaluating depending on your tolerance for licensing risk.

## Quick start

```sh
# Single-node, single-drive — instant S3 endpoint at http://localhost:9000
minio server /home/shared

# Single node with 64 local drives
minio server /mnt/data{1...64}

# Distributed, 32 nodes × 32 drives each
minio server http://node{1...32}.example.com/mnt/export{1...32}

# Distributed expanded setup
minio server \
  http://node{1...16}.example.com/mnt/export{1...32} \
  http://node{17...64}.example.com/mnt/export{1...64}

# With FTP and SFTP enabled
minio server http://node{1...4}.example.com/mnt/export{1...4} \
  --ftp="address=:8021" --ftp="passive-port-range=30000-40000" \
  --sftp="address=:8022" --sftp="ssh-private-key=$HOME/.ssh/id_rsa"
```

Useful flags (from `minio server --help`):

- `--config` — YAML server configuration (env: `MINIO_CONFIG`)
- `--address` — bind to a specific `ADDRESS:PORT`, default `:9000` (env: `MINIO_ADDRESS`)
- `--console-address` — separate `ADDRESS:PORT` for the embedded console UI (env: `MINIO_CONSOLE_ADDRESS`)
- `--certs-dir` / `-S` — path to TLS certs directory (default `~/.minio/certs`)
- `--quiet` — suppress startup banners
- `--anonymous` — hide sensitive info in logging
- `--json` — JSON-formatted logs (essential if you're shipping to anything structured)

## Client SDKs (per the MinIO server help text)

- CLI: <https://docs.min.io/docs/minio-client-quickstart-guide> (`mc`)
- Go: <https://docs.min.io/docs/golang-client-quickstart-guide>
- Java: <https://docs.min.io/docs/java-client-quickstart-guide>
- Python: <https://docs.min.io/docs/python-client-quickstart-guide>
- JavaScript: <https://docs.min.io/docs/javascript-client-quickstart-guide>
- .NET: <https://docs.min.io/docs/dotnet-client-quickstart-guide>

(In practice, any AWS SDK pointed at the MinIO endpoint works.)

## Similar / related topics

- [Ceph RADOS Gateway (RGW)](https://docs.ceph.com/en/latest/radosgw/) — heavyweight but truly open-source S3-compatible object storage on top of Ceph.
- [SeaweedFS](https://seaweedfs.com/) — Apache-2.0, lighter weight, S3-compatible.
- [Garage](https://garagehq.deuxfleurs.fr/) — small, self-hosted, AGPL-3, designed for geographically-distributed deployments.
- [Cloudflare R2](https://www.cloudflare.com/developer-platform/products/r2/) / [Backblaze B2](https://www.backblaze.com/cloud-storage) — managed S3-compatible alternatives if you don't want to self-host.
- [`mc` (MinIO Client)](https://github.com/minio/mc) — command-line client analogous to `aws s3` but with rsync-like semantics.
- [`rclone`](https://rclone.org/) — universal cloud-storage CLI; speaks S3 (and therefore MinIO) plus dozens of others.

## Internal links

<!-- reviewed -->

- [[programming/unix_systems/README|unix systems]] — parent (despite this article being about an object store, not a Unix system).
- [[programming/go/README|Go]] — MinIO is written in Go.
- [[postgresql]] — common pairing: Postgres for relational data, MinIO for blobs.

## Keywords

`#minio` `#s3` `#object-storage` `#self-hosted` `#distributed-storage` `#erasure-coding` `#golang` `#programming`

## References / raw notes

Reproduced from `minio server --help`:

```text
NAME:
  minio server - start object storage server

USAGE:
  minio server [FLAGS] DIR1 [DIR2..]
  minio server [FLAGS] DIR{1...64}
  minio server [FLAGS] DIR{1...64} DIR{65...128}

DIR:
  DIR points to a directory on a filesystem. When you want to combine
  multiple drives into a single large system, pass one directory per
  filesystem separated by space. You may also use a '...' convention
  to abbreviate the directory arguments. Remote directories in a
  distributed setup are encoded as HTTP(s) URIs.

FLAGS:
  --config value               specify server configuration via YAML configuration [$MINIO_CONFIG]
  --address value              bind to a specific ADDRESS:PORT, default ":9000" [$MINIO_ADDRESS]
  --console-address value      bind to a specific ADDRESS:PORT for embedded Console UI [$MINIO_CONSOLE_ADDRESS]
  --ftp value                  enable and configure an FTP(Secure) server
  --sftp value                 enable and configure an SFTP server
  --certs-dir value, -S value  path to certs directory (default: "~/.minio/certs")
  --quiet                      disable startup and info messages
  --anonymous                  hide sensitive information from logging
  --json                       output logs in JSON format
  --help, -h                   show help
```

Community Slack: <https://slack.min.io>
