# Hotel Management System

This is a microservices-based hotel management system that consists of four main services:

## Services

### 1. Hotel Service (Port: 8081)
- Manages hotels and rooms
- CRUD operations for hotel and room management

### 2. Reservation Service (Port: 8082)
- Handles room reservations
- Prevents double bookings using locking mechanisms
- Publishes reservation events to Kafka

### 3. Notification Service (Port: 8083)
- Consumes reservation events from Kafka
- Sends notifications (email simulations)

### 4. API Gateway (Port: 8080)
- Single entry point for all services
- Handles authentication and authorization using JWT
- Routes:
    * /api/v1/hotels/** -> Hotel Service
    * /api/v1/reservations/** -> Reservation Service
    * /api/v1/notifications/** -> Notification Service

## Prerequisites
- Docker and Docker Compose
- JDK 23
- Maven

## Running the Application

1. Clone all service repositories:
```bash
git clone https://github.com/[YOUR_USERNAME]/hotel-service.git
git clone https://github.com/[YOUR_USERNAME]/reservation-service.git
git clone https://github.com/[YOUR_USERNAME]/notification-service.git
git clone https://github.com/[YOUR_USERNAME]/api-gateway.git
```

2. Start all services using Docker Compose:
```bash
docker-compose up --build
```

## Service Dependencies
- PostgreSQL 17
- Apache Kafka & Zookeeper


## Security
- JWT-based authentication
- Token required for all endpoints except public ones
- Token format: Bearer [JWT_TOKEN]

## Testing
1. Start the services
2. Get JWT token from API Gateway
3. Use the token in other service calls

## Environment Variables
- Database configuration
    * POSTGRES_USER: hotelapp
    * POSTGRES_PASSWORD: password
    * POSTGRES_DB: hoteldb

- Kafka configuration
    * KAFKA_BOOTSTRAP_SERVERS: kafka:9092

## Troubleshooting
1. Services not starting: Check Docker logs
2. Database connection issues: Ensure PostgreSQL is running
3. Kafka connection issues: Check if Kafka and Zookeeper are healthy

## Development
- Each service can be run independently with its own docker-compose.yml
- Use docker compose up in each service directory for independent development