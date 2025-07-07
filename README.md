# Hotel Management System - Deployment

This is the main deployment repository for the **Hotel Management System** project using a microservices architecture with Docker Compose.

## Services

The system is composed of the following services:

### 1. API Gateway (`api-gateway`)
- **Description**: Routes and secures API requests using Spring Cloud Gateway.
- **Port**: `8080`
- **Security**: JWT-based authentication
- **Rate Limiting**: IP-based rate limiting enabled
- **Features**: Route forwarding, security filtering, service discovery

### 2. Auth Service (`auth-service`)
- **Description**: Handles user registration, login, and JWT token generation.
- **Port**: `8081`
- **Database**: PostgreSQL
- **Security**: BCrypt password encoding, JWT
- **Features**: 
  - `/register` and `/login` endpoints
  - Structured logging
  - Custom exception handling
  - Integration and unit tests

### 3. Hotel Service (`hotel-service`)
- **Description**: Manages hotel data such as name, location, and amenities.
- **Port**: `8082`
- **Database**: PostgreSQL
- **Features**:
  - CRUD endpoints
  - JWT-based authorization
  - Validation
  - Integration tests using Testcontainers

### 4. Reservation Service (`reservation-service`)
- **Description**: Handles room reservation logic.
- **Port**: `8083`
- **Database**: PostgreSQL
- **Messaging**: Kafka consumer for `payment-result-topic`
- **Features**:
  - Pessimistic locking
  - Custom exception handling
  - Unit & integration tests
  - Kafka retry logic
  - Authorization and mapping via DTOs

### 5. Notification Service (`notification-service`)
- **Description**: Consumes Kafka `reservation-created-topic` and logs notifications.
- **Port**: `8084`
- **Messaging**: Kafka
- **Features**: Simple consumer logic with logging

### 6. Payment Service (`payment-service`)
- **Description**: Processes payments via different providers.
- **Port**: `8085`
- **Database**: PostgreSQL
- **Messaging**: Kafka producer to `payment-result-topic`
- **Features**:
  - Strategy Pattern for provider/type selection
  - Payten mock 3D Secure integration
  - DTO-based design
  - Kafka result publishing
  - Exception handling
  - Unit tests and integration tests

## Requirements

- Docker
- Docker Compose

## Run the Project

```bash
docker compose up --build
```

> Make sure the Kafka, PostgreSQL, and services are all healthy before testing endpoints.

## Postman Collection

Use `Otellercom.postman_collection.json` to test the endpoints.

## Network

All services share the same Docker network: `hotel-network`.

## Ports Summary

| Service             | Port |
|---------------------|------|
| API Gateway         | 8080 |
| Auth Service        | 8081 |
| Hotel Service       | 8082 |
| Reservation Service | 8083 |
| Notification Service| 8084 |
| Payment Service     | 8085 |
| PostgreSQL (hotel)  | 5432 |
| PostgreSQL (auth)   | 5433 |
| PostgreSQL (hotel)  | 5434 |
| PostgreSQL (reservation) | 5435 |
| PostgreSQL (payment)| 5436 |
| Kafka               | 29092 |