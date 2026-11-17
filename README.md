# Microservices WebClient Project

This project demonstrates a **microservices architecture** using **Spring Boot** and **Spring Cloud**. It consists of two main microservices (`service-car` and `service-client`), a Discovery Server (**Eureka**), and a database infrastructure managed via **Docker Compose**.

## 🏗️ Architecture

The project is composed of the following modules:

| Module | Port | Description |
|---|---|---|
| `eureka-server` | 8761 | Service Discovery Server where all microservices register |
| `service-client` | 8081 | Manages `Client` entities (find, save, retrieve). Database: `clientservicedb` |
| `service-car` | 8082 | Manages `Car` entities (find, save, retrieve). Uses **WebClient** to call `service-client` and fetch the client details associated with each car. Database: `carservicedb` |

## 🛠️ Technologies Used

- Java 17+
- Spring Boot
- Spring Cloud (Eureka, WebClient, LoadBalancer)
- Spring Data JPA
- MySQL
- Docker & Docker Compose
- Maven

## ✅ Prerequisites

- JDK 17 or higher
- Maven
- Docker and Docker Compose (for the databases)

## 🚀 Getting Started

### 1. Start the Databases

Use Docker Compose to start the MySQL databases for both services:

```bash
docker-compose up -d
```

This will start:

- `mysql-client-service` on port **3306** (database: `clientservicedb`)
- `mysql-voiture-service` on port **3307** (database: `carservicedb`)

### 2. Start Eureka Server

```bash
cd eureka-server
mvn spring-boot:run
```

Access the Eureka Dashboard at: http://localhost:8761

### 3. Start Service Client

```bash
cd service-client
mvn spring-boot:run
```

This service registers itself with Eureka as `SERVICE-CLIENT`.

### 4. Start Service Car

```bash
cd service-car
mvn spring-boot:run
```

This service also registers with Eureka (as `SERVICE-CAR`) and can now communicate with `service-client`.

## 📡 API Endpoints

### Service Client (Port 8081)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/clients` | List all clients |
| GET | `/api/clients/{id}` | Get a client by ID |
| POST | `/api/clients` | Create a new client |

### Service Car (Port 8082)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/cars` | List all cars (enriched with client data) |
| GET | `/api/cars/byClient/{clientId}` | List cars belonging to a specific client |
| POST | `/api/cars` | Create a new car |
| GET | `/api/test/client/{id}` | Test endpoint: fetch a client directly via WebClient |

### Example Requests

```json
// POST http://localhost:8081/api/clients
{ "nom": "Salma", "age": 22 }
```

```json
// POST http://localhost:8082/api/cars
{ "marque": "Toyota", "modele": "Yaris", "clientId": 1 }
```

## ⚙️ Configuration

- `service-client` connects to `jdbc:mysql://localhost:3306/clientservicedb`
- `service-car` connects to `jdbc:mysql://localhost:3307/carservicedb`

Make sure the credentials in `application.yml` (or `application.properties`) match those in `docker-compose.yml` (default: `root` / `root`).

## 📸 Screenshots

<img width="958" height="1035" alt="img_1" src="https://github.com/user-attachments/assets/8184607e-b8cd-42e5-83f7-be114267cd20" />
<img width="827" height="192" alt="image" src="https://github.com/user-attachments/assets/b94d2d64-6f75-4ac3-bb90-620fe166c443" />
<img width="957" height="985" alt="img_2" src="https://github.com/user-attachments/assets/5dfa94b7-a2e3-4829-a509-05b42b83fc5b" />

## 👤 Author

**ECH-CHOURFI OUSSAMA**
