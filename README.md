# Magnitude Chat

Magnitude Chat is a real-time, multi-functional chat application featuring a secure authentication system, an administrative dashboard, and advanced AI bot integrations. The application is built with a modern technology stack, containerized with Docker for easy deployment, and designed for scalability.

## Project Overview

The application consists of three main components: a **FastAPI backend** that powers the core logic, a **vanilla HTML, CSS, and JavaScript frontend** for the user interface, and an **Nginx reverse proxy** that manages web traffic and serves static content. It also integrates with external services like **CoinGecko** for cryptocurrency data and **Google's Generative AI (Gemini & Vertex AI)** for conversational AI and image generation.

## Architecture

The project follows a microservices-oriented architecture, orchestrated by Docker Compose.

```mermaid
graph TD
    subgraph "User's Browser"
        A[Login Page] -->|Login Request| B(Nginx)
        C[Chat Page] -->|WebSocket| B
        D[Admin Panel] -->|API Requests| B
    end

    subgraph "Server Environment"
        B -- "Proxy Pass /auth, /chat, /admin" --> E[FastAPI Backend]
        B -- "Serve Static Files" --> F[HTML/CSS/JS]

        subgraph "FastAPI Backend Service"
            E -- "Authentication" --> G[Auth Router]
            E -- "Real-time Chat & Bots" --> H[Chat Router]
            E -- "User & Site Management" --> I[Admin Router]
            H -- "DB Operations" --> J[MongoDB]
            G -- "DB Operations" --> J
            I -- "DB Operations" --> J
        end

        subgraph "External APIs"
            H -- "@harga, @info" --> K[CoinGecko API]
            H -- "@ask" --> L[Google Gemini API]
            H -- "@generate" --> M[Google Vertex AI]
        end
    end

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style C fill:#f9f,stroke:#333,stroke-width:2px
    style D fill:#f9f,stroke:#333,stroke-width:2px
```

-   **Nginx**: Acts as the entry point for all HTTP traffic. It serves the static frontend files and reverse-proxies all API requests to the FastAPI backend.
-   **FastAPI Backend**: The core of the application, handling business logic, database interactions, and communication with external APIs.
-   **MongoDB**: The primary database, used for storing user data, chat messages, and bot conversation history.
-   **Docker**: Containerizes the Nginx and FastAPI services, ensuring a consistent and reproducible deployment environment.

## Key Features

### 1. User & Admin System

-   **Secure Authentication**: Implements JWT-based authentication with password hashing.
-   **Role-Based Access**: Differentiates between standard users and administrators, with a dedicated admin panel for management tasks.
-   **Admin Dashboard**: Allows admins to manage users (CRUD), clear chat history, delete uploaded media, and customize site content (e.g., header text, pinned messages).

### 2. Real-Time Chat

-   **WebSocket Communication**: Provides instant, bidirectional communication between clients and the server.
-   **Rich Media Support**: Users can upload and share images, videos, and other files.
-   **Message Persistence**: Chat history is saved to the database and loaded upon connection.
-   **User Presence**: Real-time notifications when users join or leave the chat.
-   **Message Actions**: Users can edit and delete their own messages.

### 3. AI Bot Integrations

SOON..

## Getting Started

### Prerequisites

-   Docker and Docker Compose
-   An environment file (`.env`) in the root directory.

### Installation & Running

1.  **Create Environment File**:
    Create a `.env` file in the project root and add the necessary environment variables (e.g., database connection strings, API keys).

    ```env
    # Example .env file
    MONGO_DETAILS="mongodb://user:pass@host:port"
    SECRET_KEY="your_jwt_secret_key"
    ALGORITHM="HS256"
    ACCESS_TOKEN_EXPIRE_MINUTES=30
    COINGECKO_API_KEY="your_coingecko_api_key"
    ```

2.  **Build and Run with Docker Compose**:
    From the project root, run the following command:

    ```bash
    docker-compose up --build
    ```

    This will build the backend image, pull the Nginx image, and start both services. The application will be accessible at `http://localhost`.


