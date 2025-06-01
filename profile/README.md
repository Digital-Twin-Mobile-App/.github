# 🌱 Digital Twin Mobile App

A full-stack mobile application for intelligent plant monitoring and management, powered by AI-based image analysis.

## 🧭 Overview

**Digital Twin Mobile App** empowers users to track plant growth, receive insights from AI, and manage plant care efficiently. The backend is built with Spring Boot, while the frontend uses React Native for cross-platform mobile support. AI services provide automated stage detection and species recognition.

---

## 🏗️ System Architecture

```
React Native App
       │
       ▼
Spring Boot Backend ───────► AI Analysis Service
       │                           ▲
       ▼                           │
 PostgreSQL, Redis, RabbitMQ ─────┘
        │
        ▼
 Google Drive (Image Storage)
```

---

## 🔁 Data Flow

1. User uploads plant image via mobile app
2. Backend stores the image in Google Drive and adds a message to RabbitMQ
3. AI service processes image asynchronously and sends results back
4. Backend updates plant data and sends notifications if needed

---

## 🚀 Features

### 🔐 Authentication & Authorization

* JWT-based security with refresh tokens
* Google OAuth2 login support
* Role-based access control (User/Admin)

### 🌿 Plant Management

* Create and manage plants with categories and images
* Track growth metrics, watering schedule, and health status
* Organize by species or custom categories

### 🖼️ Image Upload & History

* Upload from camera or gallery
* Store securely on Google Drive
* View historical timeline and metrics of growth stages

### 🤖 AI-powered Analysis

* Growth stage detection (Germination, Vegetation, Flowering)
* Plant species classification
* Height ratio estimation using contour analysis
* Confidence scores for all predictions

### 🔔 Notifications

* Stage change alerts
* Watering reminders
* System updates and plant health warnings

---

## 🧠 AI Component

### 📦 Technologies

* TensorFlow-based CNN for species detection
* OpenCV for contour-based growth analysis
* Flask or FastAPI for REST API serving predictions

### 🧪 AI Pipeline

1. Resize and normalize input image
2. Predict species via CNN
3. Analyze stage using HSV + contour methods
4. Return JSON:

```json
{
  "prediction": {
    "species": "Tomato",
    "stage": "Flowering",
    "confidence": 0.91,
    "height_ratio": 0.33
  }
}
```

---

## 🧩 Tech Stack

| Layer          | Technology                   |
| -------------- | ---------------------------- |
| **Frontend**   | React Native                 |
| **Backend**    | Spring Boot 3.x              |
| **Database**   | PostgreSQL (via Supabase)    |
| **Cache**      | Redis                        |
| **Queue**      | RabbitMQ                     |
| **AI Service** | Python + TensorFlow + OpenCV |
| **Storage**    | Google Drive API             |
| **DevOps**     | Docker + Docker Compose      |

---

## ⚙️ Configuration

Create a `.env` file in root:

```env
AI_SERVICE_URL=http://host.docker.internal:8000/predict_file/
GOOGLE_CREDENTIALS_PATH=cred.json
GOOGLE_DRIVE_FOLDER_ID=your_folder_id
SIGNER_KEY=your_jwt_signer_key
...
```

And provide your `cred.json` for Google Drive API access.

---

## 🧪 Getting Started

### Local Development

```bash
git clone https://github.com/yourusername/digital-twin-mobile-app.git
cd digital-twin-mobile-app
cp .env.example .env
./mvnw spring-boot:run
```

### Docker Deployment

```bash
docker-compose up -d
```

Includes:

* Backend
* Redis
* RabbitMQ (with delayed message support)
* AI service
* RedisInsight (optional UI)

---

## 👩‍💻 Frontend Highlights

* Secure login (JWT, Google OAuth)
* Plant dashboard & image gallery
* Visual growth timeline
* Real-time push notifications
* Responsive UI with React Navigation + Context/Redux

---

## 📜 License

Licensed under the MIT License. See `LICENSE` for more details.

---

## 🙌 Contributors

* \[Your Name or Team Name]
* AI & Backend: `Le Hoang Viet` & ` Bui Thanh Tu `
* Mobile App: `@yourmobiledev`
* Documentation & QA: `@yourteammate`
