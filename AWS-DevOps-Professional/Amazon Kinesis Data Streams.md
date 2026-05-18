# Amazon Kinesis Data Streams

Amazon Kinesis Data Streams is an AWS service widely used when real-time data processing is required.

To function properly, it requires a Producer/Consumer environment.

Supported consumers include:
- [[AWS Lambda]]
- [[Amazon Data Firehose]]
- [[Managed Service for Apache Flink]]

To insert data into a Kinesis Data Stream, you can use the AWS CLI:

*Using AWS CLI v1:*
```bash
aws kinesis put-record --stream-name <stream-name> --partition-key <partition-key> --data <data-record>
```

*Using AWS CLI v2:*
```bash
aws kinesis put-record --stream-name <stream-name> --partition-key <partition-key> --data <data-record> --cli-binary-format raw-in-base64-out
```

Where:
- **partition-key**: Defines which shard the data will be sent to.
- **data**: Defines the data payload.

For the consumer:
*To describe the data stream:*
```bash
aws kinesis describe-stream --stream-name <stream-name>
```

*To get the shard iterator:*
```bash
aws kinesis get-shard-iterator --shard-iterator-type TRIM_HORIZON --stream-name <stream-name> --shard-id <shard-id>
```

*Once you have the shard iterator from the previous command:*
```bash
aws kinesis get-records --shard-iterator <shard-iterator>
```

This represents standard Kinesis usage, without Enhanced Fan-Out (EFO).

**Kinesis Enhanced Fan-Out (EFO)** is used when there are multiple consumers reading from the same data stream. To prevent them from competing for read throughput on the shards, EFO provides each consumer with dedicated read bandwidth (up to 2 MB/s per consumer).
