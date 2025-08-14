# 🏥 Healthcare Microservices Platform (Patient Management System)

A production-grade, microservices-based platform for patient management built with Spring Boot. It features secure authentication, real-time analytics, event-driven communication, and cloud-ready infrastructure.

[![Java](https://img.shields.io/badge/Java-21-007396?logo=java)](https://adoptium.net/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.x-6DB33F?logo=spring-boot)](https://spring.io/projects/spring-boot)
[![Maven](https://img.shields.io/badge/Maven-3.9+-C71A36?logo=apache-maven)](https://maven.apache.org/)
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-Event%20Streaming-231F20?logo=apache-kafka)](https://kafka.apache.org/)
[![gRPC](https://img.shields.io/badge/gRPC-High%20Performance-00ADD8?logo=grpc)](https://grpc.io/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker)](https://www.docker.com/)
[![AWS CDK](https://img.shields.io/badge/AWS%20CDK-IaC-FF9900?logo=amazon-aws)](https://aws.amazon.com/cdk/)
[![LocalStack](https://img.shields.io/badge/LocalStack-Local%20AWS-FF9900?logo=aws)](https://localstack.cloud/)

---

## 🚀 Overview

This platform demonstrates a modern healthcare system architecture with a focus on:

- Security by design (JWT, per-service isolation)
- Reliability and scalability (stateless services, event-driven patterns)
- Developer productivity (local cloud via LocalStack, infra-as-code via AWS CDK)
- Observability and testability (integration tests, E2E flows)

Core use case: manage patients, secure access, process billing operations, and derive real-time insights.

---

## 🏗️ Architecture

### Microservices

- 🔐 Auth Service — JWT-based authentication and authorization
- 👤 Patient Service — Patient profiles and medical records (PostgreSQL)
- 💳 Billing Service — Financial operations, gRPC interactions
- 📈 Analytics Service — Real-time processing via Kafka
- 🌐 API Gateway — Centralized routing and edge concerns

### Infrastructure

- ☁️ AWS CDK — Infrastructure as Code for cloud deployments
- 🐳 Docker — Containerized services for portability
- 📡 gRPC — High-performance inter-service communication
- ✉️ Apache Kafka — Event streaming and decoupled integrations
- 🧪 LocalStack — Local AWS simulation for fast feedback loops

### High-level Diagram

```
           ┌──────────────────┐
           │    Clients       │
           │  (UI, Mobile)    │
           └────────┬─────────┘
                    │ HTTPS
             ┌──────▼───────┐
             │  API Gateway │  ← Spring Cloud Gateway
             └───┬─────┬────┘
                 │     │
     JWT/OAuth2  │     │ REST/gRPC
             ┌───▼───┐ └───────────────┐
             │ Auth  │                 │
             │Service│                 │
             └───┬───┘                 │
                 │ issues JWT          │
┌────────────────▼──────────┐     ┌────▼───────────┐
│     Patient Service       │     │  Billing Svc   │
│  (CRUD, PostgreSQL)       │ gRPC│  (Payments)    │
└──────────┬────────────────┘     └─────┬──────────┘
           │  Kafka events              │ Kafka events
           │                            │
           └────────────┬───────────────┘
                        ▼
                 Analytics Service
               (stream processing)
```

---

## 🧰 Technology Stack

- Backend: Java 21, Spring Boot 3.5.x
- Security: Spring Security, JWT
- Data: PostgreSQL (H2 for testing)
- Communication: REST, gRPC, Kafka
- Docs: OpenAPI/Swagger
- Build: Maven
- Infra: AWS CDK, LocalStack
- Containerization: Docker
- Testing: JUnit + Integration/E2E

---

## 📁 Project Structure

```
patient-management/
├── 🔐 auth-service/          # Authentication & JWT management
├── 👤 patient-service/       # Patient data & medical records
├── 💳 billing-service/       # Financial operations & gRPC server
├── 📈 analytics-service/     # Real-time analytics & Kafka consumers
├── 🌐 api-gateway/           # Spring Cloud Gateway
├── ☁️ infrastructure/        # AWS CDK stacks & local deployment scripts
├── 🧪 integration-tests/     # End-to-end tests
├── 📄 api-requests/          # REST examples (HTTP files / curl)
├── 📡 grpc-requests/         # gRPC examples
└── 🗃️ db_volumes/            # Local DB persistence (ignored in CI)
```

Tip: See each service’s README or application.yml for ports, profiles, and configuration.

---

## 🧭 Getting Started

### Prerequisites

- Java 21+
- Maven 3.9+
- Node.js 18+ (for AWS CDK)
- Docker (for local infra and optional container runs)
- Python + pip (for LocalStack CLI)
- Optional: cURL, HTTPie, or IntelliJ HTTP Client for API testing

### 1) Clone

```bash
git clone https://github.com/Matei-Gatin/healthcare-microservices-platform.git
cd healthcare-microservices-platform
```

### 2) Start Local Infrastructure (PostgreSQL, Kafka)

If you don’t have a local Kafka and PostgreSQL, you can spin them up quickly:

```yaml
# docker-compose.dev.yml
version: "3.8"
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app
      POSTGRES_DB: patients
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app -d patients"]
      interval: 5s
      timeout: 5s
      retries: 10

  kafka:
    image: bitnami/kafka:3.7
    environment:
      - KAFKA_ENABLE_KRAFT=yes
      - KAFKA_CFG_NODE_ID=1
      - KAFKA_CFG_PROCESS_ROLES=broker,controller
      - KAFKA_CFG_CONTROLLER_LISTENER_NAMES=CONTROLLER
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092,CONTROLLER://:9093
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
      - KAFKA_CFG_CONTROLLER_QUORUM_VOTERS=1@localhost:9093
    ports:
      - "9092:9092"
```

Run:
```bash
docker compose -f docker-compose.dev.yml up -d
```

Adjust DB names/ports per each service’s configuration.

### 3) Build All Services

```bash
mvn -T 1C clean verify
```

### 4) Run Locally (Dev mode)

In separate terminals:

```bash
# Example: run services with local profile (adjust to actual service names)
cd api-gateway && mvn spring-boot:run -Dspring-boot.run.profiles=local
cd auth-service && mvn spring-boot:run -Dspring-boot.run.profiles=local
cd patient-service && mvn spring-boot:run -Dspring-boot.run.profiles=local
cd billing-service && mvn spring-boot:run -Dspring-boot.run.profiles=local
cd analytics-service && mvn spring-boot:run -Dspring-boot.run.profiles=local
```

Notes:
- Look for default ports and profiles in each service’s `application.yml` or `application-*.yml`.
- Ensure services can reach PostgreSQL and Kafka (env vars or application.yml).

---

## ☁️ Local AWS (LocalStack) and IaC (AWS CDK)

The infrastructure module provides AWS CDK stacks and a script for local deployments.

1) Install LocalStack:
```bash
pip install localstack
localstack start
```

2) Install AWS CDK (if not already):
```bash
npm install -g aws-cdk
```

3) Deploy local infra:
```bash
cd infrastructure
./localstack-deploy.sh
```

This provisions local cloud dependencies (S3/SQS/etc. if used by services) in LocalStack for seamless local dev.

---

## 📚 API Documentation

Each service exposes OpenAPI/Swagger:

- Swagger UI (common paths):
  - /swagger-ui/index.html
  - /swagger-ui.html
- OpenAPI JSON:
  - /v3/api-docs

The `api-requests/` folder contains example HTTP requests for quick testing.

gRPC:
- See `grpc-requests/` for example proto calls and client usage.
- Ensure the Billing Service is running before invoking gRPC methods.

---

## 🔒 Security

- JWT-based authentication via Auth Service
- API Gateway enforces routing and edge policies
- Use HTTPS in production and secure secret storage (e.g., SSM, Secrets Manager)
- Principle of least privilege across service boundaries

---

## 🧪 Testing

- Unit tests:
  ```bash
  mvn test
  ```
- Integration/E2E tests (if a dedicated module exists):
  ```bash
  mvn -pl integration-tests -am verify
  ```

Recommended:
- Use Testcontainers for ephemeral PostgreSQL/Kafka in integration tests.
- Run services locally with the `local` profile to mirror production-like topology.

---

## 🔧 Configuration

Common areas to review per service:
- application.yml / application-<profile>.yml
- Ports and context paths
- Datasource (JDBC URL, username, password)
- Kafka bootstrap servers and topic names
- gRPC ports and target addresses
- JWT signing keys (use env vars/secrets, never commit secrets)

Example environment variables (adapt as needed):
```
SPRING_PROFILES_ACTIVE=local
DB_URL=jdbc:postgresql://localhost:5432/patients
DB_USERNAME=app
DB_PASSWORD=app
KAFKA_BOOTSTRAP_SERVERS=localhost:9092
JWT_SECRET=<strong-secret>
```

---

## 🧭 Typical Developer Workflow

1) Start infra (PostgreSQL + Kafka, optional LocalStack)
2) Build with Maven
3) Run services with `local` profile
4) Exercise APIs via `api-requests/` and gRPC via `grpc-requests/`
5) Iterate with hot reload and integration tests

---

## 🧩 Roadmap Ideas

- Add service observability (Micrometer/Prometheus/Grafana)
- Distributed tracing (OpenTelemetry)
- Saga/Outbox patterns for cross-service consistency
- Kubernetes deployment manifests/Helm charts
- Blue/Green or Canary deployments via CDK constructs

---

## 🤝 Contributing

Contributions are welcome! Please:
- Open an issue describing the change or proposal
- Keep PRs focused and well-tested
- Follow conventional commit messages where possible

---

## 📄 License

This project’s license is provided in the LICENSE file of the repository.

---

## 👤 Maintainer

- @Matei-Gatin

---

## 🧵 Appendix

- Troubleshooting:
  - Database connection refused → check `docker compose ps` and JDBC URL/credentials.
  - Kafka timeouts → verify `KAFKA_BOOTSTRAP_SERVERS` and that the broker is reachable from your host.
  - JWT validation failures → ensure services share the correct signing key/public key configuration.
- Profiles:
  - `local`: developer-friendly defaults
  - `dev`/`prod`: hardened settings for deployed environments
