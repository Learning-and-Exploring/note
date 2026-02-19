# 🚀 Notion API: Event-Driven Microservices
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-000?style=for-the-badge&logo=apachekafka&logoColor=white)](https://kafka.apache.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

A high-performance, distributed backend blueprint demonstrating **Event-Driven Architecture (EDA)**. This project leverages asynchronous messaging to decouple services and ensure high availability.

---

## 🏗 Architecture Overview

The system follows the **Database-per-Service** pattern, ensuring each microservice has complete data sovereignty and its own schema.



### Component Breakdown
| Component | Responsibility |
| :--- | :--- |
| **API Gateway** | Entry point for all clients; handles request routing and proxying. |
| **User Service** | Manages user lifecycles and triggers downstream events via Kafka. |
| **Transaction Service** | Consumes events and manages state-heavy transaction logs. |
| **Message Broker** | Apache Kafka orchestrates asynchronous communication between nodes. |

---

## 🛠 Tech Stack

* **Runtime:** Node.js (v18+) with TypeScript
* **Framework:** Express.js
* **ORM:** Prisma
* **Messaging:** Apache Kafka + Zookeeper
* **Database:** PostgreSQL 15
* **Infrastructure:** Docker & Docker Compose

---

## 🔄 Event Flow Logic

1.  **Request:** Client sends a `POST` request to the **API Gateway**.
2.  **Persistence:** **User Service** writes the primary record to its Postgres database.
3.  **Produce:** **User Service** pushes a `transaction_created` message to a Kafka topic.
4.  **Consume:** **Transaction Service** (subscriber) picks up the message from the queue.
5.  **Process:** **Transaction Service** performs secondary logic and persists the result in its own DB.

---

## 📂 Project Structure

```bash
.
├── api-gateway/            # Central entry point (Port 4000)
├── user-service/           # User logic & Kafka Producer
│   └── prisma/             # Schema for user-db
├── transaction-service/    # Business logic & Kafka Consumer
│   └── prisma/             # Schema for transaction-db
└── docker-compose.yml      # Full-stack orchestration