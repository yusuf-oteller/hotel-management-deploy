# Hotel Management System

This is a **microservices-based hotel management system** built with **Spring Boot, Docker, and Apache Kafka**.  
It provides a **scalable architecture** for managing hotels, room reservations, and notifications.

---

##  **Microservices Overview**

| Service Name             | Port | Description                                           |
|--------------------------|------|-------------------------------------------------------|
| **Hotel Service**        | 8081 | Manages hotels and rooms (CRUD operations)            |
| **Reservation Service**  | 8082 | Handles reservations and prevents double bookings     |
| **Notification Service** | 8083 | Sends notifications upon reservation events via Kafka |
| **Auth Service**         | 8084 | Auth Service                                          |
| **API Gateway**          | 8080 | Routes all requests and manages JWT authentication    |

---

##  **Prerequisites**
- **Docker & Docker Compose**
- **JDK 21 or later**
- **Maven**
- **PostgreSQL 17**
- **Apache Kafka & Zookeeper**

---

## 🚀 **Running the Application**

1. **Clone all repositories:**
   ```bash
   git clone https://github.com/yusuf-oteller/hotel-service.git
   git clone https://github.com/yusuf-oteller/reservation-service.git
   git clone https://github.com/yusuf-oteller/notification-service.git
   git clone https://github.com/yusuf-oteller/api-gateway.git
   git clone https://github.com/yusuf-oteller/auth-service.git
   git clone https://github.com/yusuf-oteller/hotel-management-deploy.git

2. **Run the services using Docker Compose::**
   ```bash
   docker-compose up -d --build

3. **Verify the services are running:**
   ```bash
   docker ps

**Note:**

- Each microservice can be run independently by navigating to its directory and using

   ```bash
   docker-compose up --build

🔐 **Security & Authentication**
--------------------------------

*   **JWT-based authentication** is implemented via the **API Gateway**.

*   Every request requires a **Bearer Token**, except for authentication endpoints.

*   The API Gateway validates the JWT token and forwards the request.


💡 **Note:**

*   **Hotel Service does NOT need SecurityConfig** because **authentication is handled at the gateway level**.


### 🔑 **Generating a JWT Token**

- To obtain a JWT token, use the **Auth Service**:

```bash
curl -X POST http://localhost:8080/api/v1/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "user@example.com", "password": "password"}'
```

- Response:
```bash
  {
    "token": "your_jwt_token_here"
  }
```

- Use this token in all further API requests:

```bash
  -H "Authorization: Bearer your_jwt_token_here"
```

# API Documentation

## Hotel Service

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/hotels` | Get all hotels | ✅ |
| POST | `/api/v1/hotels` | Create a new hotel | ✅ |
| GET | `/api/v1/hotels/{id}` | Get hotel details by ID | ✅ |
| PUT | `/api/v1/hotels/{id}` | Update hotel information | ✅ |
| DELETE | `/api/v1/hotels/{id}` | Delete a hotel | ✅ |

## Reservation Service

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/reservations` | List all reservations | ✅ |
| POST | `/api/v1/reservations` | Create a new reservation | ✅ |
| GET | `/api/v1/reservations/{id}` | Get reservation details by ID | ✅ |
| DELETE | `/api/v1/reservations/{id}` | Cancel a reservation | ✅ |

## Notification Service

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/v1/notifications` | Get all notifications | ✅ |
| GET | `/api/v1/notifications/{id}` | Get notification details by ID | ✅ |

 **Environment Variables**
-----------------------------

Set these variables in your **.env** or **Docker Compose** file:

### **Database Configuration**
```bash
  POSTGRES_USER=hotelapp
  POSTGRES_PASSWORD=password
  POSTGRES_DB=hoteldb
```

### **JWT Configuration**
```bash
  JWT_SECRET=your_secret_key
  JWT_EXPIRATION=3600000
```

 **Note:**

* **API Gateway MUST have the correct JWT_SECRET**, otherwise token verification will fail.
* To check if JWT_SECRET is set correctly inside a container:
```bash
  exec -it api-gateway printenv | grep JWT\_SECRET
```

*   If the token is **failing** (401 Unauthorized), make sure both the API Gateway and the Auth Service have the same **JWT_SECRET**.


### **Kafka Configuration**

```bash
  KAFKA_BOOTSTRAP_SERVERS=kafka:9092
```

 **Troubleshooting**
----------------------

###  **1\. Services not starting**

```bash
  logs api-gatewaydocker
  logs hotel-service
  ```

*   Ensure ports **8080-8083** are **not in use**.


###  **2\. Database connection issues**
* Run:
```bash
  docker exec -it hotel-service printenv | grep POSTGRES
  ```

* Make sure PostgreSQL is running:
```bash
  docker-compose up postgres
  ```


###  **3\. JWT Authentication Issues**

 **Common Errors:**

*   **401 Unauthorized** → API Gateway could not validate the token.

*   **403 Forbidden** → Token is valid, but user lacks permissions.


 **Steps to Debug:**
* Check if JWT_SECRET is consistent across all services:

```bash
  exec -it api-gateway printenv | grep JWT\_SECRET
  ```
* Verify the token:
```bash
  -X GET http://localhost:8080/api/v1/hotels \\ 
  -H "Authorization: Bearer your\_jwt\_token\_here"
  ```
* Inspect API Gateway logs:
```bash
  logs -f api-gateway
  ```
* Inspect Hotel Service logs:
```bash
  logs -f hotel-service
  ```


###  **4\. Kafka connection issues**
* Ensure Kafka & Zookeeper are running
```bash
  ps | grep kafka
  ```
* Restart Kafka:
```bash
  docker-compose restart kafka
  ```


**Development Notes**
-----------------------

* Each microservice has its **own** docker-compose.yml file.

* You can run services individually by navigating to the respective directory and running:

```bash
  docker-compose up --build
  ```
* Logs can be viewed using:
```bash
  docker logs -f api-gateway
  ```
