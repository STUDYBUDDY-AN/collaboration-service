# Collaboration Service

![Java](https://img.shields.io/badge/Java-25-orange?style=flat-square&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.10-brightgreen?style=flat-square&logo=springboot)
![Gradle](https://img.shields.io/badge/Gradle-Build-blue?style=flat-square&logo=gradle)
![Docker](https://img.shields.io/badge/Docker-Container-blue?style=flat-square&logo=docker)

This microservice manages study groups, real-time messaging, and file sharing within the StudyBuddy platform.

## Project Overview

*   **Purpose:** To facilitate collaboration among students through study groups, chat, and resource sharing.
*   **Key Features:**
    *   **Group Management:** Create, join, and view study groups.
    *   **Real-time Chat:** Instant messaging using WebSockets and RabbitMQ.
    *   **File Sharing:** Upload and share files (MinIO).
    *   **Authentication:** Header-based (`X-User-Id`).

## Technologies

*   **Java 25**
*   **Spring Boot 3.5.10**
*   **MySQL** (Database)
*   **Flyway** (Migrations)
*   **MinIO** (Object Storage)
*   **RabbitMQ** (Message Broker)
*   **Spring Cloud Config**
*   **Spring WebSocket**
*   **Gradle**

## Building and Running

### Prerequisites

*   **JDK 25**
*   **Docker** (for MySQL, MinIO, RabbitMQ)

### Running with Docker

1.  **Build the Docker image:**
    ```bash
    docker build -t studybuddy/collaboration-service .
    ```

2.  **Run the container:**
    ```bash
    docker run -p 8082:8082 studybuddy/collaboration-service
    ```

### Running Locally

1.  **Start Infrastructure:**
    Ensure MySQL, MinIO, and RabbitMQ are running. Use `docker-compose up -d` in the `config-repo` or project root.

2.  **Run the application:**
    ```bash
    ./gradlew bootRun
    ```

The application will be available at `http://localhost:8082`.

## Key APIs

**Base URL:** `/api/v1`

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/groups` | Create a new group. |
| `POST` | `/groups/{groupId}/join` | Join a group. |
| `GET` | `/groups/{groupId}/messages` | Get group messages. |
| `POST` | `/groups/{groupId}/messages` | Send a message. |
| `POST` | `/files/upload` | Upload a file. |
| `GET` | `/test/profile` | Get active profile and DB URL (Test endpoint). |

### WebSockets

*   **Endpoint:** `/ws`
*   **Topic:** `/topic/groups/{groupId}/events`
*   **Destinations:** `/app/chat.send.{groupId}`, `/app/chat.typing.{groupId}`

## Configuration

*   **Port:** `8082`
*   **Database:** MySQL (`jdbc:mysql://localhost:3306/collaboration_service`)
*   **Storage:** MinIO
*   **Messaging:** RabbitMQ
