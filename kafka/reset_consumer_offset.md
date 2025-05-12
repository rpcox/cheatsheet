

### Find the stalled or lagging consumer.

If unsure of which group

    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --all-groups -describe

If you know which group

    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group myConsumerGroup --describe

### Stop the consumers that will have offsets reset

### Reset the Kafka consumert offsets

####  -–to-earliest

Send the consumer back to the beginning (there will be redo's)

    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group myConsumerGroup --reset-offsets --to-earliest --topic my_topic -execute

#### --to-latest

Send the consumer to the end (a lot of data may be skipped)

    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group myConsumerGroup --reset-offsets --to-earliest --topic my_topic -execute

#### --to-datetime

Select a point in time where the consumer will begin

    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group myConsumerGroup --reset-offsets --to-datetime 2024-12-20T00:00:00.000 --topic my_topic --execute
