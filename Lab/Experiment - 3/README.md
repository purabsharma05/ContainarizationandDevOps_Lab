# Experiment 3: Deploying Web Applications with Docker

## Objective
This experiment demonstrates how to deploy a simple web application using Docker.  
A Flask-based web app is containerized using a Dockerfile, built into an image, and deployed inside a running container.

---

## Web Application Details

The application displays the following information:

- **Aakriti – SAP ID: 500119552**
- **Kinjal – SAP ID: 500123394**

---

## Technologies Used

- Python Flask
- Docker
- MacOS Terminal

---



##  Procedure 

---

### Step 1: Create Project Folder

```bash
mkdir Experiment-3-WebApp
cd Experiment-3-WebApp
```
![](./images/image1.jpeg)

### Step 2: Create Flask App (app.py)
```bash
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return """
    <h1>Experiment 3: Deploying Web Applications with Docker</h1>
    <h2>Aakriti - SAP ID: 500119552</h2>
    <h2>Kinjal - SAP ID: 500123394</h2>
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

![](./images/image2.jpeg)

### Step 3: Add Requirements File
```bash
flask
```

Create requirements.txt
![](./images/image3.jpeg)

### Step 4: Write Dockerfile
```bash
FROM python:3.9-slim

WORKDIR /app

COPY . /app

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```
![](./images/image4.jpeg)

### Step 5: Build Docker Image
```bash
docker build -t experiment3-webapp .
```
![](./images/image5.jpeg)

### Step 6: Run Docker Container
```bash
docker run -d -p 8080:5000 experiment3-webapp
```

![](./images/image6.jpeg)

![](./images/image7.jpeg)

### Step 7: Verify Deployment

Open browser:

http://localhost:8080

![](./images/image8.jpeg)


### Step 8: Check Logs
```bash
docker logs <container-id>
```

![](./images/image9.jpeg)

### Step 9: Stop the container
```bash
docker stop 7b78
1d3ca2ae
```

![](./images/image9.jpeg)

## Result

The Flask web application was successfully containerized and deployed using Docker.
It ran correctly inside a Docker container and was accessible through a web browser.

## Conclusion

Containerizing web applications ensures portability, consistency, and ease of deployment across environments.
Docker makes it simple to package and run applications anywhere without dependency issues.