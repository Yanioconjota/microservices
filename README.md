# 🧠 Ollama Microservices Architecture

This project is a microservices-based architecture integrating a FastAPI backend that communicates with Ollama to generate responses, a separate microservice to persist them into MongoDB, and a React frontend to interact with the system.

---

## 🧩 Services Overview

### 1. **FastAPI (Ollama Integration)**
- Port: `8000`
- Generates LLM-based responses via Ollama
- Forwards results to the storage microservice

### 2. **Storage Service (FastAPI + MongoDB)**
- Port: `8001`
- Saves prompt/response pairs to a MongoDB database
- Uses `httpx` for receiving data from the main API

### 3. **MongoDB**
- Port: `27018` (container internal: `27017`)
- Stores data for the storage microservice

### 4. **React Frontend (Vite + TailwindCSS)**
- Port: `5173`
- Sends prompts and displays responses using Axios

---

## 🚀 Run the Project

### 📁 Folder structure (example):
```txt
microservices/
├── docker-compose.yml
├── fast-api/
├── chat-storage-service/
└── ollama-chat/
```

### 🐳 Docker Compose
```bash
docker compose up --build
```

### 🪪 Access
- Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)
- React App: [http://localhost:5173](http://localhost:5173)

---

## 🌍 Environment Variables

### fast-api/.env
```
APP_NAME=FastAPI App
APP_ENV=development
APP_HOST=0.0.0.0
APP_PORT=8000
OLLAMA_HOST=http://host.docker.internal:11434
STORAGE_API_URL=http://storage-service:8001/save
```

### chat-storage-service/.env
```
MONGO_URL=mongodb://mongo:27017
```

---

## 🔁 Dev Flow
1. User asks something from the React frontend.
2. FastAPI receives the prompt and calls Ollama.
3. Once the response is received, it's saved by the storage service into MongoDB.
4. Response is displayed in the frontend.

---

## ⚙️ Notes
- Ollama **must be installed and running** on the host machine: [https://ollama.com](https://ollama.com)
- `extra_hosts` maps `host.docker.internal` to the host IP, so the container can reach the Ollama server.

---

## 📌 TODO
- [ ] Add loading spinners to the frontend
- [ ] Persist more metadata (timestamps, tokens, etc.)
- [ ] Add error handling / retries
- [ ] Create production Dockerfiles (multi-stage builds)

---

By Yanioconjota
