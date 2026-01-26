

    Message Queues Overview
 
    What is a Message Queue?
      A message queue is a form of asynchronous service-to-service communication used in distributed systems. Messages are stored in the          queue until they are processed and deleted.

    Key Characteristics:
     Point-to-point communication: Typically one producer and one consumer
     Ordering: Messages are usually processed in FIFO order
     Delivery semantics: Often "at-least-once" or "exactly-once"
     Persistence: Messages can be persisted or kept in memory
     Decoupling: Producers and consumers don't need to interact directly

    What is Apache Kafka?
      Kafka is a distributed event streaming platform that can handle trillions of events per day. It's designed for high throughput, fault       tolerance, and horizontal scalability.

    Key Characteristics:
      Publish-subscribe model: Multiple consumers can read messages
      Distributed architecture: Runs as a cluster of brokers
      Persistence: Stores messages on disk with configurable retention
      High throughput: Designed to handle massive message volumes
      Stream processing: Can process streams of records in real-time

    Core Kafka Concepts:
     Topics: Categories or feeds to which records are published
     Partitions: Topics are split into partitions for parallelism
     Producers: Applications that publish records to topics
     Consumers: Applications that subscribe to topics
     Brokers: Kafka servers that form the cluster
     Zookeeper: Manages cluster metadata (though being phased out in newer versions)


    RabbitMQ High-Level Design (HLD)
      RabbitMQ is an open-source message broker that implements the Advanced Message Queuing Protocol (AMQP). It provides reliable message        delivery, routing, and queuing capabilities for distributed systems.

    Core Components
     1. Broker
      Central message routing component
      Handles connection, authentication, and message routing
      Manages exchanges, queues, and bindings

     2. Exchanges
      Message routing agents that receive messages from producers
      Types:
      Direct: Routes to queues with matching routing key
      Fanout: Broadcasts to all bound queues
      Topic: Routes based on pattern matching
      Headers: Routes based on message headers

     3. Queues
      Buffers that store messages until consumed
      FIFO by default (can be prioritized)
      Can be durable, exclusive, or auto-delete

     4. Bindings
      Rules that connect exchanges to queues
      Define routing patterns between components


     [Producer] → [Exchange] → (Bindings) → [Queue] → [Consumer]
                     ↑
                [Management]
                     ↓
                [Persistence]

