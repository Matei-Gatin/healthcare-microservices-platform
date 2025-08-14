# 🏥 Patient Management System

A comprehensive microservices-based patient management system built with Spring Boot, featuring secure authentication, real-time analytics, and cloud-native infrastructure.

## 🚀 Overview

This project demonstrates a production-ready microservices architecture for healthcare management, showcasing modern software engineering practices including containerization, event-driven architecture, and Infrastructure as Code (IaC).

## 🏗️ Architecture

### Microservices

- **🔐 Auth Service** - JWT-based authentication and authorization
- **👤 Patient Service** - Core patient data management with PostgreSQL
- **💰 Billing Service** - Financial operations with gRPC communication
- **📊 Analytics Service** - Real-time data processing with Kafka
- **🌐 API Gateway** - Centralized routing and load balancing

### Infrastructure

- **☁️ AWS CDK** - Infrastructure as Code for cloud deployment
- **🐳 Docker** - Containerized microservices
- **🔧 LocalStack** - Local AWS cloud stack for development
- **📡 gRPC** - High-performance inter-service communication
- **📨 Apache Kafka** - Event streaming and message queuing

## 🛠️ Technology Stack

| Component            | Technology                 |
| -------------------- | -------------------------- |
| **Backend**          | Java 21, Spring Boot 3.5.4 |
| **Database**         | PostgreSQL, H2 (testing)   |
| **Security**         | Spring Security, JWT       |
| **Communication**    | REST APIs, gRPC, Kafka     |
| **Documentation**    | OpenAPI/Swagger            |
| **Infrastructure**   | AWS CDK, LocalStack        |
| **Containerization** | Docker                     |
| **Build Tool**       | Maven                      |
| **Testing**          | JUnit, Integration Tests   |

## 📁 Project Structure

```
patient-management/
├── 🔐 auth-service/          # Authentication & JWT management
├── 👤 patient-service/       # Patient data & medical records
├── 💰 billing-service/       # Financial operations & gRPC server
├── 📊 analytics-service/     # Real-time analytics & Kafka consumer
├── 🌐 api-gateway/          # Spring Cloud Gateway
├── ☁️ infrastructure/        # AWS CDK deployment scripts
├── 🧪 integration-tests/     # End-to-end testing suite
├── 📋 api-requests/          # API documentation & examples
├── 📡 grpc-requests/         # gRPC client examples
└── 🗄️ db_volumes/           # Local database persistence
```

## 🚦 Getting Started

### Prerequisites

- Java 21+
- Maven 3.9+
- Docker & Docker Compose
- LocalStack (for local AWS simulation)
- Node.js (for AWS CDK)

### 🔧 Local Development Setup

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd patient-management
   ```

2. **Start LocalStack**

   ```bash
   # Install LocalStack
   pip install localstack

   # Start LocalStack
   localstack start
   ```

3. **Deploy Infrastructure**

   ```bash
   cd infrastructure
   ./localstack-deploy.sh
   ```

4. **Build and Run Services**

   ```bash
   # Build all services
   mvn clean install

   # Run individual services
   cd auth-service && mvn spring-boot:run
   cd patient-service && mvn spring-boot:run
   cd billing-service && mvn spring-boot:run
   cd analytics-service && mvn spring-boot:run
   cd api-gateway && mvn spring-boot:run
   ```

### 🐳 Docker Deployment

```bash
# Build Docker images
docker build -t auth-service ./auth-service
docker build -t patient-service ./patient-service
docker build -t billing-service ./billing-service
docker build -t analytics-service ./analytics-service
docker build -t api-gateway ./api-gateway

# Run with LocalStack
docker-compose up -d
```

## 🔌 API Endpoints

### Authentication Service (Port 4005)

- `POST /auth/login` - User authentication
- `GET /auth/validate` - Token validation

### Patient Service (Port 4000)

- `GET /api/patients` - List all patients
- `POST /api/patients` - Create new patient
- `GET /api/patients/{id}` - Get patient details
- `PUT /api/patients/{id}` - Update patient
- `DELETE /api/patients/{id}` - Delete patient

### API Gateway (Port 4004)

- Centralized routing to all services
- Load balancing and rate limiting

## 📊 Monitoring & Documentation

- **Swagger UI**: Available at `http://localhost:{port}/swagger-ui.html` for each service
- **Health Checks**: `http://localhost:{port}/actuator/health`
- **Metrics**: `http://localhost:{port}/actuator/metrics`

## 🧪 Testing

```bash
# Run unit tests
mvn test

# Run integration tests
cd integration-tests
mvn test

# Run specific service tests
cd patient-service
mvn test
```

## 🔒 Security Features

- **JWT Authentication** - Stateless token-based security
- **Role-based Access Control** - Different permission levels
- **Password Encryption** - BCrypt hashing
- **API Rate Limiting** - Protection against abuse
- **CORS Configuration** - Cross-origin request handling

## 📈 Key Features

### 🔄 Event-Driven Architecture

- Kafka-based messaging
- Asynchronous processing
- Event sourcing patterns
- Microservice decoupling

## 🚀 Deployment

### Local Development

- LocalStack for AWS simulation
- Docker containers for services
- PostgreSQL databases
- Kafka message broker

### Production (AWS)

- ECS Fargate for container orchestration
- RDS PostgreSQL for databases
- MSK (Managed Kafka) for messaging
- Application Load Balancer
- Route 53 health checks

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

Gatin Matei Alexandru

- LinkedIn: Matei Alexandru Gatin
- Email: mateigatin@yahoo.com

## 🙏 Acknowledgments

- Spring Boot team for the excellent framework
- AWS for cloud infrastructure services
- LocalStack for local development tools
- Open source community for various libraries and tools

---

⭐ **Star this repository if you found it helpful!**

_This project demonstrates enterprise-level microservices architecture and is perfect for showcasing modern Java development skills in a portfolio._
