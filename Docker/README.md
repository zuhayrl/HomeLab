# Docker: What It Is, Why We Use It, and How to Get Started

Docker is a tool for building, running, and sharing containerized applications. It’s widely used because it makes development, testing, and deployment easier and more consistent across machines and environments.

---

## What Are Containers?

A **container** is a lightweight, isolated environment where you can run software with all its dependencies bundled together. Think of it like:

- A zip file that includes your code and everything it needs to run
- A mini, self-contained computer that doesn’t interfere with your actual system

Unlike full virtual machines (VMs), containers:
- Don’t contain a full operating system
- Share the host system’s kernel
- Start much faster and use fewer resources

---

## Why Use Docker?

### 1. **"It works on my machine" — Fixed**
Docker ensures your app runs **the same** everywhere:
- On your laptop
- On a teammate’s machine
- On test servers
- In production
- In the cloud

### 2. **Simplified Setup**
No more complex install instructions. Your project setup becomes:
```bash
docker build .
docker run ...
```
No need to install Python, Node.js, or databases locally — Docker handles that.

### 3. **Lightweight and Fast**
- Containers boot in seconds
- Use far fewer resources than full virtual machines
- You can run multiple containers in parallel easily

### 4. **Project isolation**
Each container:
- Has its own runtime, dependencies, and settings
- Avoids conflicts between projects (e.g., different Python versions)

### 5. **Easy to deploy anywhere**
Ship the same Docker image:
- To teammates
- In CI/CD pipelines
- To production or cloud servers
No more "it broke in prod" surprises.

### 6. **Microservices made easy**
Break your app into multiple services (API, frontend, DB) and run each in its own container.

## Tutorial
### 1. **Run Your First Container**
#### **Check if Docker is Installed**
```bash
docker --version
```

#### **Run a Test Container**
```bash
docker run hello-world
```

### 2. **Dockerise a Simple Python Project**
#### 1. Set up the project
```bash
mkdir docker-tutorial
cd docker-tutorial
```
#### 2. Create `app.py`
```python
print("Hello from inside the Docker container!")
```
#### 3. Create a `Dockerfile`
Yes, theres no file extension lmao
```bash
# Use the official Python image
FROM python:3.9

# Set working directory inside the container
WORKDIR /app

# Copy local files into the container
COPY . /app

# Define the command to run your script
CMD ["python", "app.py"]
```
#### 4. Build the docker image
```bash
docker build -t my-python-app .
```
#### 5. Run the container
```bash
docker run my-python-app
```
You should see:
```markdown
Hello from inside the Docker container!
```

### 3. **Dockerise a Flask App**
#### 1. Create `requirements.txt`
```txt
flask
```
#### 2. Create `app.py`
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Flask in Docker!"

if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0')
```
#### 3. Create `Dockerfile`
```bash
FROM python:3.9

WORKDIR /app

COPY . /app

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```
#### 4. Build the Flask Image
```bash
docker build -t flask-app .
```
#### 5. Run the container with port mapping
```bash
docker run -p 5000:5000 flask-app
```
#### 6. Open in browser to test
Visit <http://localhost:5000>
You should see:
```txt
Hello from Flask in Docker!
```
