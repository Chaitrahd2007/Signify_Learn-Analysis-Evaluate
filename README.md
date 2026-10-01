
# 🤟 SIGNIFY

### AI-Powered Gamified Sign Language Learning Platform

> **Learn Sign Language. Practice with AI. Play. Improve. Communicate.**

SIGNIFY is a gamified web platform designed to make sign language learning **accessible, engaging, and interactive** through gamification, AI-powered sign recognition, real-time camera feedback, stories, multiplayer games, and personalized learning.

---

## 📌 Table of Contents

- [🎯 Project Overview](#-project-overview)
- [✨ Key Features](#-key-features)
- [🌍 Supported Languages](#-supported-languages)
- [👥 Target Users](#-target-users)
- [🏗️ System Architecture](#️-system-architecture)
- [🛠️ Tech Stack](#️-tech-stack)
- [🤖 AI & ML Pipeline](#-ai--ml-pipeline)
- [🎮 Gamification](#-gamification)
- [📚 Learning System](#-learning-system)
- [📖 Learned Dictionary](#-learned-dictionary)
- [⚡ Speed Sign](#-speed-sign)
- [🌐 Multiplayer & Community](#-multiplayer--community)
- [🗄️ Database Architecture](#️-database-architecture)
- [🔌 Backend API](#-backend-api)
- [📁 Project Structure](#-project-structure)
- [🔐 Authentication & Security](#-authentication--security)
- [🚀 Getting Started](#-getting-started)
- [🐳 Docker Setup](#-docker-setup)
- [🧪 Testing](#-testing)
- [📈 Development Roadmap](#-development-roadmap)
- [🚀 Deployment](#-deployment)
- [♿ Accessibility](#-accessibility)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

# 🎯 Project Overview

## Mission

SIGNIFY aims to make sign language learning:

- 🎯 Accessible
- 🎮 Engaging
- 🧠 Interactive
- 🤖 AI-powered
- 📈 Progress-oriented
- 🌍 Available across multiple sign languages

The platform combines **language learning + gamification + computer vision + social learning** into a single web experience.

---

# ✨ Key Features

| Feature | Description |
|---|---|
| 🤖 AI Camera Practice | Real-time sign recognition using the user's camera |
| 🎮 Gamified Learning | XP, levels, hearts, gems, streaks and achievements |
| 📚 Structured Lessons | Units, lessons, exercises and progress tracking |
| ⚡ Speed Sign | Fast-paced timed sign recognition game |
| 📖 Story Mode | Learn signs through contextual stories |
| 🧠 Learned Dictionary | Automatically stores signs learned by the user |
| 🔁 Spaced Repetition | Smart review scheduling for learned signs |
| 🌐 Multiplayer | Real-time competitive sign language games |
| 🏆 Leaderboards | Weekly and global rankings |
| 👥 Community | Posts, comments, likes and social interaction |
| 📖 Global Dictionary | Search and explore available signs |
| 🔔 Notifications | Real-time user notifications |
| 📱 PWA Support | Installable web application experience |

---

# 🌍 Supported Languages

SIGNIFY is designed to support:

- 🇺🇸 **ASL** - American Sign Language
- 🇮🇳 **ISL** - Indian Sign Language
- 🇬🇧 **BSL** - British Sign Language

The architecture allows additional sign languages and ML models to be added later.

---

# 👥 Target Users

SIGNIFY is designed for:

- 👨‍🎓 Beginners learning sign language
- 🧑‍🏫 Students in deaf education programs
- 👨‍👩‍👧 Parents of deaf children
- 🏥 Healthcare workers
- 🌍 Anyone interested in learning sign language

---

# 🏗️ System Architecture

```text
                         ┌─────────────────────┐
                         │       USER          │
                         │   Web Browser       │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Next.js 14      │
                         │    React Frontend   │
                         └──────────┬──────────┘
                                    │
                 ┌──────────────────┼──────────────────┐
                 │                  │                  │
                 ▼                  ▼                  ▼
          ┌────────────┐     ┌────────────┐    ┌────────────┐
          │ REST API   │     │ WebSocket  │    │ Camera /   │
          │            │     │            │    │ MediaPipe  │
          └─────┬──────┘     └─────┬──────┘    └─────┬──────┘
                │                  │                  │
                ▼                  ▼                  ▼
          ┌────────────────────────────────────────────────┐
          │                  FastAPI Backend                │
          └───────────────────────┬────────────────────────┘
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
      ┌──────────────┐     ┌──────────────┐    ┌──────────────┐
      │ PostgreSQL   │     │    Redis     │    │   ML Models  │
      │              │     │ Cache / Queue│    │ MLP + GRU    │
      └──────────────┘     └──────────────┘    └──────────────┘
````

---

# 🛠️ Tech Stack

## Frontend

| Technology           | Purpose                   |
| -------------------- | ------------------------- |
| **Next.js 14**       | Web application framework |
| **TypeScript**       | Type-safe development     |
| **Tailwind CSS**     | UI styling                |
| **Framer Motion**    | Animations                |
| **Zustand**          | Global state management   |
| **TanStack Query**   | Server state management   |
| **React Hook Form**  | Form handling             |
| **Zod**              | Validation                |
| **react-webcam**     | Camera access             |
| **MediaPipe Hands**  | Hand landmark detection   |
| **Recharts**         | Progress charts           |
| **Lucide React**     | Icons                     |
| **Socket.io Client** | Real-time communication   |
| **next-pwa**         | Progressive Web App       |

---

## Backend

| Technology            | Purpose                        |
| --------------------- | ------------------------------ |
| **FastAPI**           | REST API and backend framework |
| **Python 3.11+**      | Backend language               |
| **SQLAlchemy 2.0**    | ORM                            |
| **Alembic**           | Database migrations            |
| **Pydantic v2**       | Data validation                |
| **JWT**               | Authentication                 |
| **bcrypt**            | Password hashing               |
| **Redis**             | Caching and task management    |
| **Celery**            | Background jobs                |
| **FastAPI WebSocket** | Real-time communication        |
| **Socket.io**         | Multiplayer communication      |
| **FastAPI-Mail**      | Email services                 |
| **SlowAPI**           | Rate limiting                  |

---

## AI / ML

| Technology          | Purpose                  |
| ------------------- | ------------------------ |
| **MediaPipe Hands** | Hand landmark extraction |
| **MediaPipe Pose**  | Body/pose detection      |
| **MLP**             | Static sign recognition  |
| **GRU / LSTM**      | Dynamic sign recognition |
| **TensorFlow**      | Model training           |
| **PyTorch**         | Model training           |
| **TensorFlow.js**   | Browser-based inference  |
| **ONNX Runtime**    | Server-side ML fallback  |

---

## Database & Infrastructure

| Technology                           | Purpose               |
| ------------------------------------ | --------------------- |
| **PostgreSQL 15**                    | Primary database      |
| **Redis 7**                          | Cache and queue       |
| **PostgreSQL Full-Text Search**      | Search                |
| **Meilisearch**                      | Advanced search       |
| **Cloudflare / CloudFront**          | CDN                   |
| **Vercel**                           | Frontend hosting      |
| **Railway / AWS ECS / DigitalOcean** | Backend hosting       |
| **GitHub Actions**                   | CI/CD                 |
| **Docker**                           | Containerization      |
| **Sentry**                           | Error monitoring      |
| **Prometheus**                       | Metrics               |
| **Grafana**                          | Monitoring dashboards |

---

# 🤖 AI & ML Pipeline

SIGNIFY uses computer vision and machine learning to provide real-time feedback during camera practice.

```text
Camera Feed
     │
     ▼
MediaPipe Hands + Pose
     │
     ▼
Landmark Extraction
     │
     ▼
75 × 3 Feature Representation
     │
     ├───────────────────────────┐
     │                           │
     ▼                           ▼
Static Sign                 Dynamic Sign
     │                           │
     ▼                           ▼
   MLP                     Frame Buffer
                                 │
                                 ▼
                                GRU
     │                           │
     └──────────────┬────────────┘
                    ▼
             Sign Prediction
                    │
                    ▼
             Label + Confidence
                    │
                    ▼
             User Feedback
```

---

## 🧠 Static Sign Recognition

Static signs are processed using an **MLP model**.

```text
Input
225 Features
   │
   ▼
Dense Layer - 512
   │
   ▼
Dense Layer - 256
   │
   ▼
Dense Layer - 128
   │
   ▼
Output Classes
   │
   ▼
Sign + Confidence
```

The model uses:

* 75 landmarks
* 3 coordinates per landmark
* Batch Normalization
* Dropout
* Multi-class classification

---

## 🎥 Dynamic Sign Recognition

Dynamic signs require understanding movement across multiple frames.

```text
Frame 1 ─┐
Frame 2  │
Frame 3  │
   ...   ├──► Frame Buffer
Frame 30 ┘
              │
              ▼
             GRU
              │
              ▼
        Sign Prediction
              │
              ▼
       Label + Confidence
```

The system uses a sequence of approximately **30 frames** for dynamic sign recognition.

---

# 🎮 Gamification

SIGNIFY uses game mechanics to encourage consistent learning.

## ⭐ XP System

| Action                     |            XP |
| -------------------------- | ------------: |
| Correct exercise           |         +5 XP |
| Camera exercise            |        +10 XP |
| Complete lesson            | +10 to +20 XP |
| Perfect lesson             |  +25 XP bonus |
| Complete unit              |        +50 XP |
| Complete story             |        +20 XP |
| Daily goal                 |        +10 XP |
| Win multiplayer match      |        +15 XP |
| Review learned sign        |         +3 XP |
| Speed Sign round           |  +2 to +10 XP |
| Speed Sign level           | +10 to +50 XP |
| Daily Speed Sign challenge |        +25 XP |

---

## ❤️ Hearts

```text
Maximum Hearts: 5

Wrong Answer
     │
     ▼
Lose 1 Heart

Heart Regeneration
     │
     ▼
1 Heart / 30 Minutes
```

Speed Sign uses a separate scoring system and does not consume hearts.

---

## 🔥 Streaks

Users are encouraged to practice consistently through daily streaks.

```text
Day 1 → Day 2 → Day 3 → Day 4 → ...
  🔥      🔥      🔥      🔥
```

The Speed Sign daily challenge can also contribute toward the user's streak.

---

## 💎 Gems

Gems act as an in-game currency and can be earned through activities such as:

* Daily goals
* Unit completion
* Achievements
* Special challenges
* Other game rewards

---

# 📚 Learning System

The learning system follows a structured hierarchy:

```text
Language
   │
   ▼
Units
   │
   ▼
Lessons
   │
   ▼
Exercises
   │
   ▼
Practice
   │
   ▼
Progress
```

Example:

```text
ASL
 ├── Alphabet
 │    ├── Lesson 1
 │    ├── Lesson 2
 │    └── Practice
 │
 ├── Greetings
 │    ├── Lesson 1
 │    └── Practice
 │
 ├── Numbers
 │
 ├── Family
 │
 └── Food
```

---

# 📖 Learned Dictionary

Every sign learned by the user can automatically be added to their personal **Learned Dictionary**.

The dictionary tracks:

* 📚 Learned signs
* ⭐ Starred signs
* 🧠 Mastery level
* ✅ Correct answers
* ❌ Incorrect answers
* ⏱️ Practice time
* 🔁 Review schedule
* 📝 Personal notes
* 📅 Last review date

### Mastery Levels

```text
1 ⭐       Just Learned
2 ⭐⭐      Familiar
3 ⭐⭐⭐     Practiced
4 ⭐⭐⭐⭐    Proficient
5 ⭐⭐⭐⭐⭐   Mastered
```

The system also supports **spaced repetition** for review scheduling.

---

# ⚡ Speed Sign

Speed Sign is a fast-paced sign recognition game.

```text
Sign Appears
     │
     ▼
Start Timer
     │
     ▼
User Answers
     │
     ▼
Correct?
 ┌───┴───┐
 │       │
YES      NO
 │       │
 ▼       ▼
Score    Continue
 │
 ▼
Speed Bonus
 │
 ▼
Combo Multiplier
 │
 ▼
Next Round
```

### Speed Sign Features

* ⏱️ Precision timer
* 🎯 Multiple-choice answers
* 📷 Camera answer mode
* 🔥 Combo system
* ⚡ Speed bonus
* 📈 Progressive difficulty
* 🏆 Leaderboard
* 📅 Daily challenge
* 🌐 Multiplayer mode

---

# 🌐 Multiplayer & Community

SIGNIFY includes social learning features.

## Multiplayer

Supported concepts include:

* Speed Match
* Sign Battle
* Multiplayer lobby
* Matchmaking
* Real-time score updates
* WebSocket communication
* Multiplayer Speed Sign

---

## 👥 Community

Users can:

* Create posts
* Like posts
* Comment
* Reply to comments
* View community content
* Receive notifications

---

# 🏆 Leaderboards

SIGNIFY supports:

* 🌍 Global leaderboard
* 👥 Friends leaderboard
* 📅 Weekly leaderboard
* 🏅 League system
* ⚡ Speed Sign leaderboard

League progression is based on weekly XP.

---

# 🗄️ Database Architecture

SIGNIFY uses **PostgreSQL** as its primary relational database.

Major database systems include:

```text
USER SYSTEM
├── users
├── user_sessions
└── user_friends

CONTENT SYSTEM
├── sign_languages
├── units
├── lessons
├── signs
└── lesson_signs

EXERCISE SYSTEM
└── exercises

PROGRESS SYSTEM
├── user_unit_progress
├── user_lesson_progress
├── user_exercise_attempts
└── user_sign_mastery

GAMIFICATION
├── achievements
├── user_achievements
├── daily_xp_log
├── heart_transactions
└── gem_transactions

SOCIAL
├── leagues
├── league_seasons
├── league_participants
├── multiplayer_matches
└── match_players

STORY
├── stories
├── story_scenes
└── user_story_progress

COMMUNITY
├── community_posts
├── post_comments
└── post_likes

DICTIONARY
├── dictionary_bookmarks
├── user_learned_signs
└── user_dictionary_stats

SPEED SIGN
├── speed_sign_levels
├── speed_sign_sessions
├── speed_sign_rounds
├── speed_sign_leaderboard
└── speed_sign_daily_challenges
```

---

# 🔌 Backend API

The backend is organized using versioned REST APIs.

```text
/api/v1
│
├── /auth
├── /users
├── /units
├── /lessons
├── /exercises
├── /signs
├── /progress
├── /gamification
├── /achievements
├── /leaderboard
├── /social
├── /multiplayer
├── /stories
├── /community
├── /notifications
├── /camera
├── /dictionary
├── /admin
└── /games/speed-sign
```

Real-time features use WebSockets / Socket.io.

---

# 📁 Project Structure

## Backend

```text
signify-backend/
│
├── app/
│   ├── models/
│   ├── schemas/
│   ├── api/
│   │   └── v1/
│   ├── services/
│   ├── core/
│   ├── ml/
│   │   └── models/
│   ├── websocket/
│   ├── tasks/
│   └── utils/
│
├── alembic/
├── tests/
├── scripts/
├── data/
│
├── main.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── alembic.ini
├── pyproject.toml
└── .env.example
```

---

## Frontend

```text
signify-frontend/
│
├── public/
│   ├── icons/
│   ├── images/
│   ├── sounds/
│   ├── lottie/
│   └── ml-models/
│
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   ├── (main)/
│   │   └── api/
│   │
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   ├── camera/
│   │   ├── dictionary/
│   │   ├── games/
│   │   ├── gamification/
│   │   ├── leaderboard/
│   │   ├── social/
│   │   └── shared/
│   │
│   ├── hooks/
│   ├── stores/
│   ├── lib/
│   │   ├── ml/
│   │   └── game-engines/
│   │
│   ├── services/
│   ├── types/
│   └── styles/
│
├── package.json
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── middleware.ts
```

---

# 🔐 Authentication & Security

SIGNIFY uses:

* 🔑 JWT authentication
* 🔒 bcrypt password hashing
* 🔐 Protected API routes
* 👤 User authorization
* 🛡️ Rate limiting
* 🌐 CORS configuration
* 🔒 Secure environment variables
* 🧱 Database constraints
* 📊 Monitoring with Sentry

### Authentication Flow

```text
User
 │
 ├── Register
 │
 ▼
Backend
 │
 ▼
Password Hash
 │
 ▼
PostgreSQL
 │
 ▼
JWT Access Token
 │
 ▼
Authenticated Requests
```

---

# 🚀 Getting Started

## Prerequisites

Install the following:

* Node.js 18+
* Python 3.11+
* PostgreSQL 15+
* Redis 7+
* Git
* Docker (recommended)

---

## 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/signify.git

cd signify
```

---

## 2. Backend Setup

```bash
cd signify-backend

python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### Linux / macOS

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create environment file:

```bash
cp .env.example .env
```

Run migrations:

```bash
alembic upgrade head
```

Start FastAPI:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Backend will be available at:

```text
http://localhost:8000
```

Swagger documentation:

```text
http://localhost:8000/docs
```

---

# 3. Frontend Setup

```bash
cd signify-frontend

npm install
```

Create:

```text
.env.local
```

Example:

```env
NEXT_PUBLIC_APP_NAME=SIGNIFY
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
NEXT_PUBLIC_WS_URL=ws://localhost:8000
NEXT_PUBLIC_ML_MODEL_BASE_URL=/ml-models
```

Start the development server:

```bash
npm run dev
```

Frontend will be available at:

```text
http://localhost:3000
```

---

# 🐳 Docker Setup

The project supports Docker-based development.

Start the services:

```bash
docker compose up -d
```

Check running containers:

```bash
docker compose ps
```

Stop the services:

```bash
docker compose down
```

---

# 🧪 Testing

## Backend

```bash
pytest
```

Run a specific test:

```bash
pytest tests/test_auth.py
```

---

## Frontend

```bash
npm test
```

---

## E2E Testing

The project roadmap includes Playwright-based end-to-end testing.

```bash
npx playwright test
```

---

# 📈 Development Roadmap

## Phase 1: Foundation

* [x] Next.js frontend
* [x] FastAPI backend
* [x] PostgreSQL setup
* [x] Redis setup
* [x] Authentication
* [x] Basic UI components
* [x] Core content system
* [x] Exercise system

---

## Phase 2: Gamification

* [x] XP system
* [x] Level system
* [x] Hearts
* [x] Streaks
* [x] Gems
* [x] Achievements
* [x] Leaderboards

---

## Phase 3: AI Camera Practice

* [x] MediaPipe integration
* [x] Landmark extraction
* [x] Static sign recognition
* [x] Dynamic sign recognition
* [x] GRU model integration
* [x] ISL model support
* [x] Server-side ML fallback
* [x] Camera practice progress tracking
* [x] Web Worker optimization

---

## Phase 4: Learned Dictionary

* [x] Learned signs database
* [x] Automatic sign addition
* [x] Mastery tracking
* [x] Spaced repetition
* [x] Review queue
* [x] Personal notes
* [x] Star / unstar
* [x] Dictionary statistics

---

## Phase 5: Speed Sign

* [x] Speed Sign levels
* [x] Timed rounds
* [x] Scoring system
* [x] Combo system
* [x] Speed bonuses
* [x] Daily challenges
* [x] Leaderboards
* [x] Multiplayer Speed Sign

---

## Phase 6: Social & Multiplayer

* [x] WebSocket multiplayer
* [x] Matchmaking
* [x] Speed Match
* [x] Sign Battle
* [x] Community posts
* [x] Likes and comments
* [x] Friend system
* [x] Real-time notifications

---

## Phase 7: Story Mode & Dictionary

* [x] Story mode
* [x] Interactive story scenes
* [x] Story-based sign practice
* [x] Global dictionary
* [x] Full-text search
* [x] Bookmark system
* [x] Additional exercises
* [x] Additional sign content

---

## Phase 8: Polish & Launch

* [x] Backend unit tests
* [x] Frontend component tests
* [x] E2E testing
* [x] Load testing
* [x] Accessibility audit
* [x] Mobile responsiveness
* [x] Security audit
* [x] CI/CD
* [x] Production deployment
* [x] Beta testing
* [x] Public launch

---

## Phase 9: Post-Launch

* [x] Analytics and A/B testing
* [x] Improve ML models
* [x] More learning content
* [x] More stories
* [x] Speed Sign events
* [x] Dictionary export/import
* [x] Enhanced accessibility
* [x] Multiple UI languages
* [x] Additional sign language models
* [x] Future mobile application

---

# 🚀 Deployment

### Frontend

Recommended:

```text
Vercel
```

### Backend

Possible deployment options:

```text
Railway
AWS ECS
DigitalOcean
```

### Database

```text
PostgreSQL
```

### Cache / Queue

```text
Redis
```

### CDN

```text
Cloudflare
CloudFront
```

### Monitoring

```text
Sentry
Prometheus
Grafana
```

### CI/CD

```text
GitHub Actions
```

---

# ♿ Accessibility

SIGNIFY is designed with accessibility in mind.

Supported configuration includes:

* 🔊 Sound controls
* 📳 Haptic feedback
* 🌓 Dark mode
* 🔲 High contrast
* 🔠 Adjustable text size
* 📱 Responsive layouts
* 🎥 Camera-based interaction
* 🧭 Keyboard-friendly navigation

---

# ⚙️ Environment Variables

## Backend

Create:

```text
.env
```

Example:

```env
APP_NAME=SIGNIFY
APP_ENV=development
DEBUG=true
API_VERSION=v1

HOST=0.0.0.0
PORT=8000
WORKERS=4

DATABASE_URL=postgresql+asyncpg://postgres:password@localhost:5432/signify

REDIS_URL=redis://localhost:6379/0

JWT_SECRET_KEY=your-super-secret-key
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=15
REFRESH_TOKEN_EXPIRE_DAYS=7

GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

AWS_ACCESS_KEY_ID=your-aws-key
AWS_SECRET_ACCESS_KEY=your-aws-secret
AWS_REGION=us-east-1
S3_BUCKET_NAME=signify-media

SENTRY_DSN=your-sentry-dsn

RATE_LIMIT_PER_MINUTE=100

MAX_HEARTS=5
HEART_REFILL_MINUTES=30

ML_MODEL_PATH=./app/ml/models
ML_CONFIDENCE_THRESHOLD=0.7
```

> ⚠️ Never commit `.env` files or API keys to GitHub.

---

# 📊 Core Data Flow

```text
                    USER
                     │
                     ▼
              ┌─────────────┐
              │  Next.js UI │
              └──────┬──────┘
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
     REST API              WebSocket
          │                     │
          └──────────┬──────────┘
                     ▼
              ┌─────────────┐
              │   FastAPI   │
              └──────┬──────┘
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
 PostgreSQL        Redis       ML Engine
       │             │             │
       └─────────────┼─────────────┘
                     ▼
              User Feedback
```

---

# 🎯 Project Goals

SIGNIFY focuses on combining:

```text
Sign Language
      +
Gamification
      +
Artificial Intelligence
      +
Computer Vision
      +
Personalized Learning
      +
Social Interaction
```

The goal is to create a learning environment where users can **learn signs, practice them, receive AI feedback, track progress, compete with others, and build long-term learning habits.**

---

# 🤝 Contributing

Contributions are welcome!

### 1. Fork the repository

```bash
git clone https://github.com/YOUR_USERNAME/signify.git
```

### 2. Create a feature branch

```bash
git checkout -b feature/your-feature
```

### 3. Commit your changes

```bash
git add .
git commit -m "feat: add your feature"
```

### 4. Push the branch

```bash
git push origin feature/your-feature
```

### 5. Open a Pull Request

Please describe:

* What was changed
* Why it was changed
* How it was tested
* Any screenshots or demo videos if applicable

---

# 📄 License

This project is currently under development.

Add your preferred license here, for example:

```text
MIT License
```
# ⭐ Support

If you find SIGNIFY interesting:

⭐ Star the repository
🍴 Fork the project
🐛 Report issues
💡 Suggest features
🤝 Contribute to development

---

<p align="center">

### 🤟 Learn • Practice • Play • Communicate

**SIGNIFY**

Developers:
- For phase 2-Chaitra