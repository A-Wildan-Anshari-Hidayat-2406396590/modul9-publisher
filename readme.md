### a. How much data will your publisher program send to the message broker in one run?

The publisher program sends **5 events** (messages) to the message broker in a single run. Each message is a `UserCreatedEventMessage` that includes a `user_id` (String) and a `user_name` (String). The five messages are published to the `user_created` queue with the following data:

| user_id | user_name        |
|---------|------------------|
| 1       | 2406396590-Amir  |
| 2       | 2406396590-Budi  |
| 3       | 2406396590-Cica  |
| 4       | 2406396590-Dira  |
| 5       | 2406396590-Emir  |

### b. The URL of `amqp://guest:guest@localhost:5672` is the same as in the subscriber program. What does it mean?

It means that both the publisher and subscriber connect to the **same RabbitMQ message broker** instance. They use the same connection URL because they need to communicate through the same intermediary. The publisher sends messages to this broker, while the subscriber listens for messages from that broker. This setup is central to the event-driven architecture pattern— the publisher and subscriber do not talk to each other directly. Instead, they both connect to a shared message broker that routes messages from producers to consumers.

### Running RabbitMQ as message broker

![RabbitMQ Management UI](assets/rabbitmq.png)