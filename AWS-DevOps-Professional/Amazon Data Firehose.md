# Amazon Data Firehose

Amazon Data Firehose (formerly Amazon Kinesis Data Firehose) can ingest data from:
- [[Amazon Kinesis Data Streams]]
- [[Amazon CloudWatch]]
- AWS IoT

All records can be up to 1 MB in size.

It supports data transformation before processing using an [[AWS Lambda]] function (e.g., converting CSV to JSON).

It can forward batch writes to destinations such as:
- [[Amazon S3]]
- [[Amazon Redshift]]
- Amazon OpenSearch Service
- HTTP custom endpoints
- Third-party services like Splunk, Datadog, New Relic, MongoDB

You can configure it to write all data or just failed records to an [[Amazon S3]] bucket.

Unlike Kinesis Data Streams, Firehose is a **near-real time** service.

It supports the following data formats:
- CSV
- Parquet
- JSON
- Avro
- Raw Text
- Binary data

## Kinesis Data Streams vs. Amazon Data Firehose

| Feature | Kinesis Data Streams | Amazon Data Firehose |
| --- | --- | --- |
| **Use Case** | Stream data collection, real-time processing | Loading data into S3/Redshift/OpenSearch/HTTP endpoints/3rd party services |
| **Latency** | Real-time | Near-real time |
| **Throughput** | Scalable (1 MB/s - 1 GB/s) | Scalable (200 KB/s - 12 MB/s) |
| **Ordering** | Ordered within partition with replay capability | Not ordered and no replay capability |
| **Transformations** | Custom code (Lambda, KCL) | Built-in Lambda transformations |
| **Destinations** | Custom (S3, Redshift, OpenSearch, etc.) | S3, Redshift, OpenSearch, HTTP endpoints, Splunk, Datadog, New Relic, MongoDB |
