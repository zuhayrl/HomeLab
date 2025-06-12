# Docker: What It Is, Why We Use It, and How to Get Started

Docker is a tool for building, running, and sharing containerized applications. It’s widely used because it makes development, testing, and deployment easier and more consistent across machines and environments.

**Table of Contents**

* [What Are Containers?](#What-Are-Containers?)
* [Why Use Docker?](#Why-Use-Docker?)
* [Docker Tutorial](#Docker-Tutorial)
* [Dockerfile](#Dockerfile)

---
# What Are Containers?
  A **container** is a lightweight, isolated environment where you can run software with all its dependencies bundled together. Think of it like:
  
  - A zip file that includes your code and everything it needs to run
  - A mini, self-contained computer that doesn’t interfere with your actual system
  
  Unlike full virtual machines (VMs), containers:
  - Don’t contain a full operating system
  - Share the host system’s kernel
  - Start much faster and use fewer resources

---
# Why Use Docker?
  ## 1. **"It works on my machine" — Fixed**
  Docker ensures your app runs **the same** everywhere:
  - On your laptop
  - On a teammate’s machine
  - On test servers
  - In production
  - In the cloud
  
  ## 2. **Simplified Setup**
  No more complex install instructions. Your project setup becomes:
  ```bash
  docker build .
  docker run ...
  ```
  No need to install Python, Node.js, or databases locally — Docker handles that.
  
  ## 3. **Lightweight and Fast**
  - Containers boot in seconds
  - Use far fewer resources than full virtual machines
  - You can run multiple containers in parallel easily
  
  ## 4. **Project isolation**
  Each container:
  - Has its own runtime, dependencies, and settings
  - Avoids conflicts between projects (e.g., different Python versions)
  
  ## 5. **Easy to deploy anywhere**
  Ship the same Docker image:
  - To teammates
  - In CI/CD pipelines
  - To production or cloud servers
  No more "it broke in prod" surprises.
  
  ## 6. **Microservices made easy**
  Break your app into multiple services (API, frontend, DB) and run each in its own container.

---
  # Docker Tutorial
  ## Part 1. **Run Your First Container**
  ### **Check if Docker is Installed**
  ```bash
  docker --version
  ```
  
  ### **Run a Test Container**
  ```bash
  docker run hello-world
  ```
  
  ## Part 2. **Dockerise a Simple Python Project**
  ### 1. Set up the project
  ```bash
  mkdir docker-tutorial
  cd docker-tutorial
  ```
  ### 2. Create `app.py`
  ```python
  print("Hello from inside the Docker container!")
  ```
  ### 3. Create a `Dockerfile`
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
  ### 4. Build the docker image
  ```bash
  docker build -t my-python-app .
  ```
  ### 5. Run the container
  ```bash
  docker run my-python-app
  ```
  You should see:
  ```markdown
  Hello from inside the Docker container!
  ```
  
  ## Part 3. **Dockerise a Flask App**
  ### 1. Create `requirements.txt`
  ```txt
  flask
  ```
  ### 2. Create `app.py`
  ```python
  from flask import Flask
  
  app = Flask(__name__)
  
  @app.route('/')
  def home():
      return "Hello from Flask in Docker!"
  
  if __name__ == "__main__":
      app.run(debug=True, host='0.0.0.0')
  ```
  ### 3. Create `Dockerfile`
  ```bash
  FROM python:3.9
  
  WORKDIR /app
  
  COPY . /app
  
  RUN pip install -r requirements.txt
  
  EXPOSE 5000
  
  CMD ["python", "app.py"]
  ```
  ### 4. Build the Flask Image
  ```bash
  docker build -t flask-app .
  ```
  ### 5. Run the container with port mapping
  ```bash
  docker run -p 5000:5000 flask-app
  ```
  ### 6. Open in browser to test
  Visit <http://localhost:5000>
  You should see:
  ```txt
  Hello from Flask in Docker!
  ```
  
  ## Part 4. **Intercative Applications**
  Say you would like to pass input into your application - this would be an **interactive** application.
  
  **Example: `app.py`:**
  ```python
  x = input("Enter text: ")
  print("You entered:", x)
  ```
  **Example `Dockerfile`:**
  ```bash
  FROM python:3.9
  
  WORKDIR /app
  
  COPY . .
  
  CMD ["python", "app.py"]
  ```
  **Build the docker image:**
  ```bash
  docker build -t input-example .
  ```
  **Running the docker image (IMPORTNAT):**
  
  To make input() work, you must run Docker with:
  - -i: keep STDIN open
  - -t: allocate a pseudo-TTY (gives you the terminal interface)
   
  So use:
  ```
  docker run -it input-example
  ```
  
  # Useful Docker Commands
  ```bash
  docker ps             # Show running containers
  docker ps -a          # Show all containers (including stopped)
  docker images         # List all images
  docker stop <id>      # Stop a container
  docker rm <id>        # Remove a container
  docker rmi <image>    # Remove an image
  ```

---
# Dockerfile
  A `dockerfile` is a plain text file (i.e. no extension) that contains instructions to build a docker image.  
  ## Example:
  ```
  # 1. Base image
  FROM python:3.9
  
  # 2. Set working directory inside the container
  WORKDIR /app
  
  # 3. Copy project files into the container
  COPY . /app
  
  # 4. Install dependencies
  RUN pip install -r requirements.txt
  
  # 5. Expose the port your app runs on (optional, for web apps)
  EXPOSE 5000
  
  # 6. Command to run your app
  CMD ["python", "app.py"]
  ```
  
  ## **Breakdown**
  | Line                  | What It Does                                                |
  | --------------------- | ----------------------------------------------------------- |
  | `FROM python:3.9`     | Sets the base image (like the OS + Python installed)        |
  | `WORKDIR /app`        | Creates and switches to `/app` inside the container         |
  | `COPY . /app`         | Copies all local files into the container's `/app` folder   |
  | `RUN pip install ...` | Runs a command (e.g., installs Python packages)             |
  | `EXPOSE 5000`         | Documents the port the app listens on (useful for web apps) |
  | `CMD [...]`           | The default command that runs when the container starts     |
  
  ## Rules and Tips
  - Every Dockerfile must start with FROM — this sets the base image.
  
  - Each instruction creates a new layer — Docker caches these layers to speed up builds.
  
  - Use COPY (or ADD) to bring in your code or files.
  
  - Use RUN for any commands that should be executed when building the image (e.g., installing packages).
  
  - Use CMD to set the command that runs when you run the container.
  
  You can also use:
  
  - ENV to set environment variables
  
  - ARG for build-time variables
  
  - ENTRYPOINT to define how a container starts (often combined with CMD)
