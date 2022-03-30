# Command
- ### Create topic
    ```
    kafka-topics.sh --create --bootstrap-server <hostname>:9092 --replication-factor 3 --partitions 3 --topic <topic>
    ```
- ### Delete topic
    ```
    kafka-topics.sh --delete --bootstrap-server <hostname>:9092 --topic <topic>
    ```
- ### List topic
    ```
    kafka-topics.sh --list --zookeeper <hostname>:2181
    ```
- ### Check replication and partitions topic
    ```
    kafka-topics.sh --zookeeper <hostname>:2181 --topic <topic> --describe
    ```
- ### Producer
    ```
    kafka-console-producer.sh --broker-list <hostname>:9092 --topic <topic>
    ```
- ### Consumer
    ```
    kafka-console-consumer.sh --bootstrap-server <hostname>:9092  --topic <topic> --from-beginning

    kafka-console-consumer.sh --bootstrap-server <hostname>:9092  --topic <topic> 
    ```
- ### List consumer groups
    ```
    kafka-consumer-groups.sh  --list --bootstrap-server <hostname>:9092
    ```
- ### Check consumer groups
    ```
    kafka-consumer-groups.sh --describe --group <mygroup> --bootstrap-server <hostname>:9092
    ```

- ### Consume data per partition
    ```
    kafka-console-consumer.sh --bootstrap-server <hostname>:9092 --topic <topic> --partition 0 --from-beginning
    ```
- ### Purge a topic

    ```
    kafka-topics.sh --zookeeper <hostname>:2181 --alter --topic <topic> --config retention.ms=100
    ```
    #### รอประมาณ 1-2 นาที 
    ```
    kafka-topics.sh --zookeeper <hostname>:2181 --alter --topic <topic> --delete-config retention.ms
    ```

    - all topic use = ".*"

- ### Increse Topic Replication Factor
    1. Specify the extra replicas in a custom reassignment json file
    ```shell
    vi increase-replication-factor.json
    ```
    ```shell
    {"version":1,
        "partitions":[
            {"topic":"<topic>","partition":0,"replicas":[0,1,2]},
            {"topic":"<topic>","partition":1,"replicas":[0,1,2]},
            {"topic":"<topic>","partition":2,"replicas":[0,1,2]}
    ]}
    ```
    2. Use the file with the --execute option of the kafka-reassign-partitions tool
    ```shell
    kafka-reassign-partitions.sh --zookeeper localhost:2181 --reassignment-json-file increase-replication-factor.json --execute
    ```
    3. Verify the replication factor with the kafka-topics tool
    ```shell
    kafka-topics.sh --zookeeper localhost:2181 --topic <topic> --describe
    ```
