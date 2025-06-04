
# 📦 Scalable Event-Driven Microservices Architecture with Kafka

This project demonstrates a distributed, event-driven microservices architecture implemented in Python using [Apache Kafka](https://kafka.apache.org/) for real-time communication between services.

Each microservice performs a specific role and communicates asynchronously through Kafka topics to ensure scalability, fault tolerance, and loose coupling.

---

## 🧱 Microservices Overview

### 1. `order-service`
Generates new customer orders using the `faker` library and sends them to a Kafka topic (`order-details`).

- **Tech:** Python, KafkaProducer, Faker
- **Responsibility:** Simulate real-time order creation
- **Topic Produced:** `order-details`

---

### 2. `transaction-service`
Consumes orders from `order-details`, calculates total cost, and sends processed transactions to a new topic (`order-processed`).

- **Tech:** Python, KafkaConsumer, KafkaProducer
- **Responsibility:** Handle order transaction logic
- **Topic Consumed:** `order-details`  
- **Topic Produced:** `order-processed`

---

### 3. `notification-service`
Listens to the `order-processed` topic and simulates sending email notifications to customers.

- **Tech:** Python, KafkaConsumer
- **Responsibility:** Email alert on successful orders
- **Topic Consumed:** `order-processed`

---

### 4. `analytics-service`
Tracks real-time analytics including total orders and revenue by listening to the `order-processed` topic.

- **Tech:** Python, KafkaConsumer
- **Responsibility:** Real-time metrics and insights
- **Topic Consumed:** `order-processed`

---

## ⚙️ Architecture Diagram

          +----------------+
          |  Order Service |
          +----------------+
                |
                v
    +---------------------------+
    | Kafka Topic: order-details|
    +---------------------------+
                |
                v
      +----------------------+
      | Transaction Service  |
      +----------------------+
                |
                v
      +-----------------------------+
      | Kafka Topic: order-processed|
      +-----------------------------+
               |              |
               v              v
    +----------------+  +----------------+
    | Notification   |  |   Analytics    |
    |    Service     |  |    Service     |
    +----------------+  +----------------+

Flow Explanation:

🛒 Order Service generates mock order events and sends them to Kafka topic order-details.

💰 Transaction Service consumes these, processes them, calculates total cost, and sends output to order-processed.

📬 Notification Service listens to order-processed and simulates sending emails.

📊 Analytics Service consumes the same topic and aggregates revenue and order count.

