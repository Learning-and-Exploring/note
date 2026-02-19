# 📝 Notion — Full-Stack Note App

[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-000?style=for-the-badge&logo=apachekafka&logoColor=white)](https://kafka.apache.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)

A full-stack note-taking application built on an **Event-Driven Microservices** architecture, with a React frontend served through Nginx.

---

## 🏗 Architecture Overview

```
                       ┌──────────────────────────────────────────────────┐
                       │                Docker Network                    │
                       │                                                  │
                       │                                                  │
    ┌─────────┐        │  ┌───────────┐         ┌───────────────┐         │
    │         │        │  │   Nginx   │         │  API Gateway  │         │
    │ Browser ├── :80 ─┼─►│ (UI/Proxy)├────────►│    (Nodejs)   │         │
    │         │        │  │           │         │     :4000     │         │
    └─────────┘        │  └───────────┘         └───────┬───────┘         │
                       │                                │                 │
                       │                ┌───────────────┴──────┐          │
                       │        ┌───────▼───────┐      ┌───────▼──────┐   │
                       │        │  User Service │      │ Note Service │   │
                       │        │     :4001     │      │     :4002    │   │
                       │        └───────┬───────┘      └───────┬──────┘   │
                       │                │                      │          │
                       │        ┌───────▼───────┐      ┌───────▼──────┐   │
                       │        │    PostgreSQL │      │  PostgreSQL  │   │
                       │        │   (user-db)   │      │  (note-db)   │   │
                       │        └───────┬───────┘      └───────┬──────┘   │
                       │                │                      │          │
                       │        ┌───────▼──────────────────────▼──────┐   │
                       │        │          Apache Kafka / ZK          │   │
                       │        │           (Event Bus)               │   │
                       │        └─────────────────────────────────────┘   │
                       └──────────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```bash
.
├── api/                        # Backend microservices
│   ├── api-gateway/            # Entry point — routes requests (Port 4000)
│   ├── user-service/           # User management + Kafka Producer (Port 4001)
│   ├── note-service/           # Note management + Kafka Consumer (Port 4002)
│   └── docker-compose.yml      # Full infrastructure orchestration
├── ui/                         # React 18 + TypeScript frontend
│   └── README.md
└── nginx/                      # Reverse proxy
    ├── Dockerfile
    └── nginx.conf
```

---

## 🛠 Tech Stack

| Layer              | Technology                         |
| :----------------- | :--------------------------------- |
| **Frontend**       | React 18, TypeScript, Vite         |
| **API Gateway**    | Node.js, Express, TypeScript       |
| **Services**       | Node.js, Express, TypeScript       |
| **Message Broker** | Apache Kafka + Zookeeper           |
| **Database**       | PostgreSQL 16 (one DB per service) |
| **Reverse Proxy**  | Nginx                              |
| **Infrastructure** | Docker & Docker Compose            |

---

## 🔄 Event Flow

1. Client sends a request → **Nginx** (`:80`)
2. Nginx proxies `/api/*` → **API Gateway** (`:4000`)
3. API Gateway routes to **User Service** or **Note Service**
4. Service writes to its own **PostgreSQL** database
5. Service publishes an event to **Kafka**
6. Subscribing service consumes the event and performs secondary logic

---

## 🚀 Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) & Docker Compose v2

### Run the full stack

```bash
cd api/
docker compose up --build
```

| URL                     | Description             |
| :---------------------- | :---------------------- |
| `http://localhost`      | UI (via Nginx)          |
| `http://localhost/api`  | API Gateway (via Nginx) |
| `http://localhost:4000` | API Gateway (direct)    |
| `http://localhost:4001` | User Service (direct)   |
| `http://localhost:4002` | Note Service (direct)   |

---

## 📖 Service READMEs

- [API Services →](./api/README.md)
- [UI →](./ui/README.md)
