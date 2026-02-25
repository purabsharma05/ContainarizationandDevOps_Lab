## EXPERIMENT - 4

---


## Docker Essentials


# Part 1: Containerizing Applications with Dockerfile

## Step 1: Create a Simple Application

### Python Flask App

```bash
mkdir my-flask-app
cd my-flask-app
```

![Setup](IMG - 01.png)


### app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Docker!"

@app.route('/health')
def health():
    return "OK"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

![Setup](IMG - 02.png)


### requirements.txt

```
Flask==2.3.3
```

![Setup](IMG - 03.png)


---

## Step 2: Create Dockerfile

```dockerfile
# Use Python base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements file
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Expose port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

![Setup](IMG - 04.png.png)


---

# Part 2: Using .dockerignore

## Step 1: Create .dockerignore File

```bash
# Python cache files
__pycache__/
*.pyc

# Virtual environments
.env
.venv
env/
venv/

# IDE files
.vscode/
.idea/

# Git files
.git/
.gitignore

# Logs
*.log
logs/
```

![Setup](IMG - 05.png)


## Why .dockerignore is Important

- Prevents unnecessary files from being copied  
- Reduces image size  
- Improves build speed  
- Increases security  

---

# Part 3: Building Docker Images

## Step 1: Basic Build

```bash
docker build -t my-flask-app .
docker images
```

![Setup](IMG - 06.png)


## Step 2: Tagging Images

```bash
docker build -t my-flask-app:1.0 .
docker tag my-flask-app:latest my-flask-app:v1.0
docker tag my-flask-app:latest username/my-flask-app:1.0
```

![Setup](IMG - 07.png)


## Step 3: View Image Details

```bash
docker images
docker history my-flask-app
docker inspect my-flask-app
```

---

# Part 4: Running Containers

## Step 1: Run Container

```bash
docker run -d -p 5000:5000 --name flask-container my-flask-app
```

![Setup](IMG - 08.png)


Test application:

```bash
curl http://localhost:5000
```
![Setup](IMG - 09.png)

Check running containers:

```bash
docker ps
docker logs flask-container
```

![Setup](IMG - 10.png)


---

## Step 2: Manage Containers

```bash
docker stop flask-container
docker start flask-container
docker rm flask-container
docker rm -f flask-container
```

---

# Part 5: Multi-stage Builds

## Why Multi-stage Builds?

- Smaller final image size  
- Better security  
- Separate build and runtime environment  

## Example Multi-stage Dockerfile

```dockerfile
# STAGE 1: Builder
FROM python:3.9-slim AS builder

WORKDIR /app
COPY requirements.txt .
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --no-cache-dir -r requirements.txt

# STAGE 2: Runtime
FROM python:3.9-slim

WORKDIR /app
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY app.py .

RUN useradd -m -u 1000 appuser
USER appuser

EXPOSE 5000
CMD ["python", "app.py"]
```

![Setup](IMG - 11.png)


## Build and Compare

```bash
docker build -t flask-regular .
docker build -f Dockerfile.multistage -t flask-multistage .
docker images | grep flask
```

![Setup](IMG - 11.png)

![Setup](IMG - 11.png)


Expected:
- flask-regular → ~250MB  
- flask-multistage → ~150MB  

---

# Part 6: Publishing to Docker Hub

## Login & Tag

```bash
docker login
docker tag my-flask-app:latest username/my-flask-app:1.0
docker push username/my-flask-app:1.0
```

![Setup](IMG - 11.png)

![Setup](IMG - 11.png)


## Pull on Another Machine

```bash
docker pull username/my-flask-app:latest
docker run -d -p 5000:5000 username/my-flask-app:latest
```

![Setup](images/16.png)


---

# Part 7: Node.js Example

## Node.js App (app.js)

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    res.send('Hello from Node.js Docker!');
});

app.get('/health', (req, res) => {
    res.json({ status: 'healthy' });
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

![Setup](images/17.png)


## Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install --only=production

COPY app.js .

EXPOSE 3000
CMD ["node", "app.js"]
```

![Setup](images/18.png)


![Setup](images/19.png)


## Build & Run

```bash
docker build -t my-node-app .
docker run -d -p 3000:3000 --name node-container my-node-app
curl http://localhost:3000
```

![Setup](images/20.png)

![Setup](images/21.png)


---

# Essential Docker Commands Cheatsheet

| Command | Purpose | Example |
|----------|----------|----------|
| docker build | Build image | docker build -t myapp . |
| docker run | Run container | docker run -p 3000:3000 myapp |
| docker ps | List containers | docker ps -a |
| docker images | List images | docker images |
| docker tag | Tag image | docker tag myapp:v1 myapp:v2 |
| docker push | Push image | docker push username/myapp |
| docker pull | Pull image | docker pull username/myapp |
| docker rm | Remove container | docker rm container-name |
| docker rmi | Remove image | docker rmi image-name |
| docker logs | View logs | docker logs container-name |
| docker exec | Execute command | docker exec -it container-name bash |

---

# Common Workflow Summary

## Development Workflow

```bash
docker build -t myapp .
docker run -p 8080:8080 myapp
docker tag myapp:latest myapp:v1.0
docker push myapp:v1.0
```


## Production Workflow

```bash
docker pull myapp:v1.0
docker run -d -p 80:8080 --name prod-app myapp:v1.0
docker logs -f prod-app
```

![Setup](images/22.png)


![Setup](images/23.png)

---

# Key Takeaways

- Dockerfile defines how to build your image  
- .dockerignore improves performance and security  
- Tagging helps version management  
- Multi-stage builds reduce image size  
- Docker Hub enables sharing images  
- Always test locally before publishing  

---

# Cleanup

```bash
docker container prune
docker image prune
docker system prune -a
```

![Setup](images/24.png)