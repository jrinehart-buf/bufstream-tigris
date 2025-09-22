# bufstream-tigris

[Bufstream](https://buf.build/docs/bufstream/) is a fully self-hosted drop-in replacement for Apache Kafka® that writes data to S3-compatible object storage. It’s 100% compatible with the Kafka protocol, including support for exactly-once semantics (EOS) and transactions. Bufstream is more cost-effective to operate, and a single cluster can elastically scale to hundreds of GB/s of throughput without sacrificing performance. It's the universal Kafka replacement for the modern age.

Even better, for teams sending Protobuf messages across their Kafka topics, Bufstream can enforce data quality and governance requirements on the broker with Protovalidate. Bufstream can even store topics as Apache Iceberg™ tables, reducing time-to-insight in popular data lakehouse products like Snowflake and ClickHouse.

To interact with Bufstream, we’ll use Kafkactl, a CLI tool for interacting with Apache Kafka and compatible tools.
In addition, we’ll use Docker, the universal package format for the Internet. Docker lets you put your application and all its dependencies into a container image so that it can’t conflict with anything else on the system.


## Prerequisites

Docker Desktop or another similar app like Podman Desktop.

## Steps 

1. Create a Tigris bucket in the standard tier.
2. [Create an access key](https://console.tigris.dev/createaccesskey) and note the ID and secret. Give it "Editor" permissions for your bucket.
3. Update .env with your Tigris key ID and secret:
    ```
     TIGRIS_ACCESS_KEY_ID=tid_
     TIGRIS_SECRET_ACCESS_KEY=tsec_
    ```
4. In bufstream/config/bufstream.yaml, update storage.bucket to use the bucket name.
5. Start the environment, using -d to return to the prompt:

    `docker compose up -d`

6. Create a topic:

    `docker exec cli kafkactl create topic bufstream-on-tigris`

7. Produce messages to the topic (note: example uses a canned set of messages in messages.txt):

    `docker exec cli kafkactl produce bufstream-on-tigris --file=/messages.txt`

8. Consume messages:

    `docker exec cli kafkactl consume bufstream-on-tigris --tail=100`
