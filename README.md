
# Digital Twin Mobile App Backend

A Spring Boot application for plant monitoring and management with AI-powered plant stage detection.

## Overview

This application serves as the backend for a Digital Twin Mobile App that allows users to track and monitor plants. It uses AI to detect plant stages, species, and growth metrics from uploaded images. The system provides notifications for plant stage changes and maintains a history of plant images and their analysis.

## System Architecture

The application follows a microservices-inspired architecture with the following components:

```
┌─────────────────┐     ┌─────────────────────┐     ┌─────────────────┐
│                 │     │                     │     │                 │
│  React Native   │────▶│    Spring Boot      │────▶│  AI Analysis    │
│  Mobile App     │◀────│    Backend          │◀────│  Service        │
│                 │     │                     │     │                 │
└─────────────────┘     └─────────────────────┘     └─────────────────┘
                               │      ▲
                               │      │
                               ▼      │
                        ┌─────────────────────┐
                        │                     │
                        │  PostgreSQL, Redis  │
                        │  RabbitMQ, GDrive   │
                        │                     │
                        └─────────────────────┘
```

### Data Flow Diagram

```
┌──────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐
│          │    │           │    │           │    │           │    │           │
│  Upload  │───▶│  Temp     │───▶│ RabbitMQ  │───▶│   AI      │───▶│  Update   │
│  Image   │    │  Storage  │    │  Queue    │    │ Analysis  │    │ Database  │
│          │    │           │    │           │    │           │    │           │
└──────────┘    └───────────┘    └───────────┘    └───────────┘    └───────────┘
                      │                                                   │
                      ▼                                                   ▼
                ┌───────────┐                                      ┌───────────┐
                │           │                                      │           │
                │  Google   │                                      │  User     │
                │  Drive    │                                      │ Notif.    │
                │           │                                      │           │
                └───────────┘                                      └───────────┘
```

### Component Interaction Diagram

```
┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│                │     │                │     │                │
│ Controllers    │────▶│  Services      │────▶│ Repositories   │
│                │◀────│                │◀────│                │
└────────────────┘     └────────────────┘     └────────────────┘
       │  ▲                   │  ▲                  │  ▲
       │  │                   │  │                  │  │
       ▼  │                   ▼  │                  ▼  │
┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│                │     │                │     │                │
│ DTOs/Mappers   │     │ RabbitMQ       │     │ Database       │
│                │     │ Components     │     │ (PostgreSQL)   │
└────────────────┘     └────────────────┘     └────────────────┘
                              │  ▲
                              │  │
                              ▼  │
                       ┌────────────────┐
                       │                │
                       │ External AI    │
                       │ Service        │
                       │                │
                       └────────────────┘
```

## Features

### User Authentication and Authorization

The system provides secure user authentication through:

- **JWT-based Authentication**: Secure token-based authentication with configurable expiration times
- **Google OAuth2 Integration**: Allows users to sign in with their Google accounts
- **Token Refresh**: Automatic token refresh mechanism to maintain user sessions
- **Role-based Authorization**: Different access levels for regular users and administrators

### Plant Management

Users can manage their plants through:

- **Plant Creation**: Create new plants with names, categories, and cover images
- **Plant Monitoring**: Track plant growth and health over time
- **Plant Categorization**: Organize plants by species and categories
- **Watering Schedule**: Set and manage watering frequency for each plant

### Image Upload and Processing

The system handles plant images through:

- **Multi-format Support**: Upload images in various formats (JPEG, PNG)
- **Secure Storage**: Images are securely stored in Google Drive
- **Asynchronous Processing**: Image uploads don't block the user interface
- **Thumbnail Generation**: Automatic creation of thumbnails for faster loading

### AI-powered Plant Analysis

The AI component provides:

- **Plant Stage Detection**: Automatically identifies growth stages:
  - **Germination**: Early sprouting stage
  - **Vegetation**: Leaf development stage
  - **Flowering**: Reproductive stage
- **Species Identification**: Recognizes plant species from images
- **Height Ratio Measurement**: Calculates plant height relative to image size
- **Confidence Scoring**: Provides confidence levels for all predictions

### Real-time Notifications

Users receive notifications for:

- **Stage Changes**: Alerts when a plant enters a new growth stage
- **Health Issues**: Warnings about potential plant health problems
- **Watering Reminders**: Notifications based on watering schedule
- **System Updates**: Information about new features and improvements

### Image History Tracking

The system maintains a comprehensive history:

- **Chronological Timeline**: View plant growth over time
- **Growth Metrics**: Track changes in height and development
- **Stage Transitions**: Record when plants move between growth stages
- **Comparison Tools**: Compare current and past plant states

## Technology Stack

- **Backend**: Spring Boot 3.x
- **Database**: PostgreSQL (via Supabase)
- **Caching**: Redis
- **Message Queue**: RabbitMQ with delayed message exchange
- **Authentication**: JWT, OAuth2
- **File Storage**: Google Drive
- **Containerization**: Docker
- **AI Integration**: External AI service via REST API
- **Frontend**: React Native

## Prerequisites

- JDK 17 or higher
- Maven
- Docker and Docker Compose
- Google Drive API credentials

## Configuration

The application uses environment variables for configuration. Create a `.env` file in the root directory with the following variables:

```
# Environment
ENV=dev
DDL_UPDATE=update
AI_SERVICE_URL=http://host.docker.internal:8000/predict_file/

# Backend
BACKEND_HOST=localhost
BACKEND_PORT=8082
BACKEND_DOCKER_PORT=8080

# Drive
GOOGLE_CREDENTIALS_PATH=cred.json
GOOGLE_DRIVE_FOLDER_ID=your_folder_id

# PostgreSQL
POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DB=postgres
POSTGRES_HOST=your_postgres_host
POSTGRES_PORT=5432

# Redis
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_TIMEOUT=60000

# RabbitMQ
RABBITMQ_HOST=rabbitmq
RABBITMQ_PORT=5672

# JWT
SIGNER_KEY=your_jwt_signer_key

# Google OAuth
GOOGLE_ID=your_google_client_id
GOOGLE_PASSWORD=your_google_client_secret
GOOGLE_REDIRECT_URI=http://localhost:8082/login/oauth2/code/google

# Email Config
EMAIL=your_email@gmail.com
APP_PASSWORD=your_app_password
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_SMTP_AUTH=true
MAIL_SMTP_STARTTLS_ENABLE=true
```

## Getting Started

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/digital-twin-mobile-app.git
   cd digital-twin-mobile-app
   ```

2. Create a `.env` file with your configuration (see above)

3. Place your Google Drive API credentials in `cred.json`

4. Run the application:
   ```bash
   ./mvnw spring-boot:run
   ```

### Docker Deployment

Build and run using Docker Compose:
   ```bash
   docker-compose up -d
   ```

This will start the following services:
- Backend application
- RabbitMQ with delayed message exchange
- Redis
- RedisInsight (Redis management UI)

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributors

- Your Team Members

## Acknowledgments

- Spring Boot
- RabbitMQ
- Redis
- PostgreSQL
- Docker