# 🚀 Prompt Hub

**Prompt Hub** is a full-stack web application for discovering, saving, liking, and managing useful AI prompts for tools such as **ChatGPT, Gemini, and other Large Language Models (LLMs)**.

The platform provides a centralized collection of optimized prompts for **developers, creators, students, and AI enthusiasts**, making it easier to discover, copy, save, and reuse prompts for real-world tasks.

The project is built using the **MERN Stack** with **Redis caching**, authentication, Docker, and cloud services.

---

## 🌐 Live Demo

🔗 **Frontend:**
https://prompt-hub.vercel.app

🔗 **Backend API:**
https://prompt-hub-lxak.onrender.com

> Replace the frontend URL above with your current Vercel URL if it is different.

---

## ✨ Features

### 🧠 AI Prompt Collection

* Browse a collection of useful AI prompts.
* Prompts designed for coding, productivity, content creation, image generation, and problem solving.
* Organized structure for quick discovery.

### 🔐 Authentication

* User login and authentication.
* User-specific data management.
* Protected actions for authenticated users.

### ❤️ Like System

* Users can like prompts.
* Like count is updated dynamically.
* Prevents duplicate favorites for the same user.

### ⭐ Favorites

* Users can save/like prompts to their personal favorites.
* Favorites can be retrieved separately.
* User-specific favorites are cached using Redis.

### ⚡ Redis Caching

Redis is used to improve application performance by reducing repeated database queries.

Caching is implemented for:

* Latest prompts
* Individual prompt data
* User favorites

Example caching flow:

```text
Request
   ↓
Redis
   ↓
 ┌───────────────┐
 │ Cache HIT     │ → Return cached data
 └───────────────┘

        OR

 ┌───────────────┐
 │ Cache MISS    │
 └───────┬───────┘
         ↓
      MongoDB
         ↓
    Store in Redis
         ↓
      Response
```

### 🗑️ Cache Invalidation

When prompt data changes, the related Redis cache is invalidated or updated.

For example:

```text
Create Prompt
     ↓
MongoDB
     ↓
Delete prompts cache
     ↓
Next request → MongoDB
     ↓
New data cached in Redis
```

This keeps cached data synchronized with the database.

### 📋 Copy Prompts

* Easily copy prompts.
* Modify prompts according to your requirements.
* Use them with ChatGPT, Gemini, or other AI tools.

### 🖼️ Prompt Images

* Supports prompt-related images.
* Images are stored using cloud object storage.

### 🐳 Docker Support

The complete application is containerized using Docker.

Separate Docker environments are available for:

* Frontend
* Backend

Docker Compose is used to run the application locally.

```bash
docker compose up --build
```

### ☁️ Cloud Deployment

The project uses cloud services for production:

* **Frontend:** Vercel
* **Backend:** Render
* **Database:** MongoDB Atlas
* **Redis:** Upstash Redis

---

# 🛠️ Tech Stack

## Frontend

* React.js
* JavaScript
* HTML5
* CSS3
* React Hooks
* REST API
* Nginx
* Docker

## Backend

* Node.js
* Express.js
* MongoDB
* Mongoose
* Redis
* REST API
* Docker

## Authentication

* Custom authentication flow
* Token-based authentication
* User-specific favorites

## Database & Caching

* MongoDB Atlas
* Upstash Redis

## DevOps / Deployment

* Docker
* Docker Compose
* Nginx
* GitHub
* Vercel
* Render

## Storage

* Cloudflare R2

---

# 📸 Screenshots

## 🏠 Home Page

<img
src="https://github.com/user-attachments/assets/27d3eca0-041f-455c-aa22-f0e3ce4b8746"
alt="Prompt Hub Home Page"
width="900"
/>

## 📝 Prompt Details

<img
src="https://github.com/user-attachments/assets/82ae3c1b-e2c3-4c2c-9642-60cc8f7f17fe"
alt="Prompt Hub Prompt Details"
width="700"
/>

---

# 📁 Project Structure

```text
prompt-hub/
│
├── backend/
│   ├── config/
│   │   └── redis.js
│   │
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── server.js
│   ├── package.json
│   └── package-lock.json
│
├── frontend/
│   ├── public/
│   ├── src/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── nginx.conf
│   ├── package.json
│   └── package-lock.json
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

# 🔌 API Endpoints

## Prompts

### Get Latest Prompts

```http
GET /api/prompts
```

Returns the latest prompts.

Redis caching is used to reduce repeated MongoDB queries.

### Create Prompt

```http
POST /api/prompts
```

Creates and stores a new prompt.

The API also invalidates the prompts cache after successful creation.

---

## ❤️ Likes

### Like Prompt

```http
PATCH /api/prompts/:id/like
```

Updates the prompt's like count and associates the prompt with the authenticated user.

Redis is used to update the individual prompt cache and invalidate the user's favorites cache.

---

## ⭐ Favorites

### Get User Favorites

```http
GET /api/users/favorites
```

Returns the authenticated user's favorite prompts.

Favorites are cached using a user-specific Redis key:

```text
favorites:<userId>
```

---

## 🔐 Authentication

### Login

```http
POST /api/auth/login
```

Creates or retrieves a user and returns authentication information.

### Current User

```http
GET /api/auth/me
```

Returns the authenticated user's information.

---

# ⚡ Redis Implementation

Redis is integrated as a caching layer between the application and MongoDB.

### Prompt Cache

```text
prompts:latest
```

### Individual Prompt Cache

```text
prompt:<promptId>
```

### User Favorites Cache

```text
favorites:<userId>
```

Example Redis flow:

```text
Frontend
   ↓
Express API
   ↓
Redis
   ├── HIT  → Return cached data
   │
   └── MISS
        ↓
      MongoDB
        ↓
   Cache in Redis
        ↓
     Response
```

This helps reduce database load and improves response time for frequently requested data.

---

# 🐳 Running With Docker

## Prerequisites

Make sure you have installed:

* Docker Desktop
* Git

MongoDB and Redis do not need to run locally because the application can connect to:

* MongoDB Atlas
* Upstash Redis

---

## 1. Clone Repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
```

```bash
cd prompt-hub
```

---

## 2. Configure Environment Variables

Create:

```text
backend/.env
```

Example:

```env
MONGODB_URI=your_mongodb_atlas_connection_string

UPSTASH_REDIS_REST_URL=your_upstash_redis_url

UPSTASH_REDIS_REST_TOKEN=your_upstash_redis_token
```

Create:

```text
frontend/.env
```

For local development:

```env
REACT_APP_API_URL=http://localhost:5000
```

> Never commit `.env` files or secret credentials to GitHub.

---

## 3. Build and Start Containers

From the project root:

```bash
docker compose up --build
```

The application will start the frontend and backend containers.

Frontend:

```text
http://localhost:3001
```

Backend:

```text
http://localhost:5000
```

---

## 4. Stop Containers

```bash
docker compose down
```

To temporarily stop containers:

```bash
docker compose stop
```

To start them again:

```bash
docker compose start
```

---

# 🐳 Docker Architecture

```text
                 Docker Compose
                       │
              ┌────────┴────────┐
              ↓                 ↓
        Frontend Container   Backend Container
             Nginx              Node.js
           Port 80            Port 5000
              │                 │
              └────────┬────────┘
                       ↓
              External Services
                ┌──────┴──────┐
                ↓             ↓
          MongoDB Atlas   Upstash Redis
```

---

# ☁️ Production Architecture

```text
                    Users
                      │
                      ↓
               Vercel Frontend
                      │
                      ↓
               Render Backend
                      │
              ┌───────┴────────┐
              ↓                ↓
        MongoDB Atlas      Upstash Redis
              │
              ↓
        Persistent Data
```

---

# 🔄 Development Workflow

```text
Developer
    ↓
Write Code
    ↓
Test Locally
    ↓
Docker Compose
    ↓
Frontend + Backend
    ↓
MongoDB Atlas + Upstash
    ↓
Git Commit
    ↓
GitHub
    ↓
Cloud Deployment
```

---

# 🎯 Project Goals

This project was built to:

* Practice full-stack development.
* Understand REST API development.
* Learn Redis caching and cache invalidation.
* Implement authentication and user-specific data.
* Build a real-world prompt management platform.
* Learn Docker and Docker Compose.
* Work with cloud databases and caching services.
* Deploy a full-stack application to the cloud.
* Improve understanding of scalable backend architecture.

---

# 📚 What I Learned

Through this project, I worked with:

* MERN stack architecture
* REST API design
* MongoDB and Mongoose
* Redis caching
* Redis cache invalidation
* Authentication
* User-specific data
* Docker containers
* Docker Compose
* Nginx
* Environment variables
* MongoDB Atlas
* Upstash Redis
* Cloudflare R2
* Vercel deployment
* Render deployment
* Git and GitHub

---

# 🚀 Future Improvements

Some possible future improvements:

* 🔍 Advanced prompt search
* 🏷️ Prompt tags and categories
* 👍 Like/unlike toggle
* 🔐 Improved authentication using JWT/OAuth
* 📊 User dashboard
* 📈 Analytics
* 🔔 Notifications
* 🧠 AI-powered prompt recommendations
* 🗂️ Better prompt filtering and sorting
* 🧪 Automated testing
* ⚙️ CI/CD pipeline

---

# 👨‍💻 Author

## Bhumika Sahu

**Full Stack Web Developer**

Passionate about building modern full-stack applications, AI-powered tools, and scalable web solutions.

### Tech Interests

* MERN Stack
* Redis
* Docker
* Cloud Deployment
* AI Tools
* Backend Development

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.
