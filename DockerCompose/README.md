# Docker Compose
## Table of Contents


# What is Docker Compose?

Docker Compose is a tool that lets you **define and run multi-container Docker applications** using a single file: `docker-compose.yml`.

Instead of starting each container manually, Compose lets you:

- Define your entire stack  
- Launch all services with one command  
- Share consistent environments across teams

---

# Why Use Docker Compose?

| Feature | Benefit |
|--------|---------|
| Single config file | Define web, database, cache, etc. together |
| Easy setup | Just run `docker-compose up` |
| Scalable | Run multiple replicas with `--scale` |
| Reproducible | Works the same across machines and environments |
| Manages networks/volumes | Built-in support without manual setup |

---

# Tutorial Project

This guide walks you through using **Docker Compose** to run a simple multi-container application consisting of:
- A **Flask web server**
- A **Redis** backend

You’ll build a small web app that:

- Uses **Flask** to serve a webpage
- Stores a counter in **Redis**
- Increments the counter on each page visit

---

## Project Structure
docker-compose-tutorial/  
│  
├── app/  
│ ├── app.py  
│ └── requirements.txt  
│  
├── Dockerfile  
└── docker-compose.yml  

---

## Create the App

### Step 1. Flask App Code: `app/app.py`

```python
from flask import Flask
import redis

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = cache.incr('hits')
    return f'Hello! This page has been viewed {count} times.\n'
```

### Step 2. Python Requirements: `app/requirements.txt`
```txt
flask
redis
```

### Step 3. Dockerfile (for Flask app):
```Dockerfile
FROM python:3.9

WORKDIR /app

COPY app/ /app

RUN pip install -r requirements.txt

CMD ["python", "app.py"]

```

### Step 4. Docker Compose File: `docker-compose.yml`
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: "redis:alpine"

```

---

## Run the App

### Step 1: Build and Start Everything
From the root folder (where `docker-compose.yml` is), run:
```bash
docker-compose up --build
```
This will:
- Build the Flask image from the Dockerfile
- Pull the Redis image from Docker Hub
- Create a network and start both services

### Step 2: Open in Browser
Go to: <http://localhost:5000>
Each refresh will increment the Redis counter.

### Step 3: Stop the App
Use `Ctrl+C` to stop.
Then clean up everything with:
```bash
docker-compose down
```

---

## Making Changes
If you modify any Python code or dependencies:
```bash
docker-compose up --build
```
This forces a rebuild.

---

## Scaling with Multiple Containers
Want to run 3 instances of the Flask app (load-balanced):
```bash
docker-compose up --scale web=3
```
You’ll still see a single counter, since all containers talk to the same Redis service.

---

## .dockerignore
To keep the image clean, create a `.dockerignore` file:
```
__pycache__
*.pyc
*.log
.env
```

---

# Summary of Docker Compose Commands

| Command                     | Description                           |
| --------------------------- | ------------------------------------- |
| `docker-compose up`         | Start services (build if needed)      |
| `docker-compose up --build` | Force rebuild of images               |
| `docker-compose down`       | Stop and clean up containers/networks |
| `docker-compose ps`         | Show running containers               |
| `docker-compose logs`       | View logs for all services            |
| `--scale web=3`             | Run multiple instances of a service   |

