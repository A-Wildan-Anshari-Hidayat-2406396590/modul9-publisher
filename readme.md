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

### Program Outputs

**Subscriber Terminal:**
![Subscriber Output](assets/cargo-run-subscriber.png)

**Publisher Terminal:**
![Publisher Output](assets/cargo-run-publisher.png)

**What is happening:**  
When we run `cargo run` in the publisher directory, the publisher program connects to the RabbitMQ broker and pushes 5 `UserCreatedEventMessage` events into the `user_created` queue. Because the subscriber program is actively listening to this exact same queue, it instantly pulls these 5 messages from the broker and prints them to its console. This sequence demonstrates the core of event-driven architecture: the publisher and subscriber are completely independent processes, but they can seamlessly communicate in real-time by passing messages through the RabbitMQ broker.

### Simulating a Slow Subscriber

![RabbitMQ Queued Messages Spike](assets/rabbitmq-performance.png)

**Why does the spike happen?**
By adding a 1-second `thread::sleep` delay into the subscriber, we artificially slowed down its processing speed. When we rapidly ran the publisher multiple times, we instantly flooded the message broker with dozens of events. Because the publisher's speed far exceeded the subscriber's processing rate (1 message/second), the RabbitMQ message broker had to hold those unprocessed messages in the queue, creating a massive spike on the "Queued messages" chart. The line then slowly slopes downward as the slow subscriber gradually works through the backlog, one message at a time. This perfectly illustrates why message queues are so useful: they act as a buffer to prevent a slow consumer from crashing under the pressure of a high-speed producer!