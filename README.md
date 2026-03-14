# kafka-python-demo

![Kafka](https://img.shields.io/badge/Apache-Kafka-black?logo=apachekafka)
![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Docker](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)

A minimal **Apache Kafka event streaming pipeline** implemented using
**Python producers/consumers** and **Docker Compose**.

This repository demonstrates the fundamental Kafka architecture:

Producer → Kafka Topic → Consumer

It provides a **minimal reference implementation** for exploring event
streaming concepts and running Kafka locally without requiring dedicated
infrastructure.

------------------------------------------------------------------------

## Architecture

    Python Producer
          │
          ▼
    Kafka Topic (orders)
          │
          ▼
    Python Consumer

Kafka runs inside Docker using the **Confluent Kafka image in KRaft
mode** (no ZooKeeper required).

------------------------------------------------------------------------

## Local Development Environment

This project was developed and tested on a **local virtual machine**:

-   VirtualBox
-   Ubuntu 22.04
-   4 GB RAM
-   2 CPU
-   Docker + Docker Compose

The goal of this setup is to demonstrate that Kafka event streaming
pipelines can be developed and explored locally without requiring cloud
infrastructure or dedicated servers.

------------------------------------------------------------------------

## Project Structure

    kafka-python-demo
    ├── docker-compose.yaml
    ├── producer.py
    └── tracker.py

  File                    Description
  ----------------------- --------------------------------------
  `docker-compose.yaml`   Starts a Kafka broker in Docker

  `producer.py`           Sends order events to Kafka
  
  `tracker.py`            Consumes events from the Kafka topic

------------------------------------------------------------------------

## Prerequisites

-   Docker
-   Docker Compose
-   Python 3
-   `confluent-kafka` Python library

Install dependency:

``` bash
pip install confluent-kafka
```

------------------------------------------------------------------------

## Start Kafka

Start Kafka with Docker Compose:

``` bash
docker compose up -d
```

Verify the container is running:

``` bash
docker ps
```

------------------------------------------------------------------------

## Run the Producer

The producer publishes events to the **orders** topic.

``` bash
python3 producer.py
```

Example output:

    Delivered {"order_id": "...", "user": "lara", "item": "frozen yogurt", "quantity": 10}
    Delivered to orders : partition 0 : at offset 1

------------------------------------------------------------------------

## Inspect Kafka Topics

List topics:

``` bash
docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092
```

Describe the topic:

``` bash
docker exec -it kafka kafka-topics --bootstrap-server localhost:9092 --describe --topic orders
```

------------------------------------------------------------------------

## Consume Messages Using Kafka CLI

``` bash
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic orders --from-beginning
```

------------------------------------------------------------------------

## Run the Python Consumer

``` bash
python3 tracker.py
```

Example output:

    Consumer is running and subscribed to orders topic
    Received order: 10 x frozen yogurt from lara
    Received order: 4 x pizza from alex

------------------------------------------------------------------------

## Key Kafka Concepts Demonstrated

-   Event producers publishing messages to a topic
-   Kafka storing events as an append-only log
-   Consumers reading events using offsets
-   Consumers catching up on events produced while they were offline
-   Kafka acting as a buffer between systems

------------------------------------------------------------------------

## Inspiration

This project was inspired by the Kafka hands-on tutorial by **TechWorld
with Nana**:

https://www.youtube.com/watch?v=B7CwU_tNYIE

------------------------------------------------------------------------

## License

This repository is provided as a **minimal reference implementation** of
a Kafka event streaming pipeline using Python and Docker.
