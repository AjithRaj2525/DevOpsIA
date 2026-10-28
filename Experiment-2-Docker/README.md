
# Experiment 2: Docker Containerization

## Aim
To containerize a simple web application using Docker.

## Requirements
- Docker Desktop
- Visual Studio Code
- A web browser

## Files
- `index.html` - Contains the HTML webpage.
- `Dockerfile` - Defines how to build the Docker image.

## Dockerfile
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

## Procedure

### 1. Verify Docker
```bash
docker --version
docker info
```

### 2. Build the image
```bash
docker build -t devops-web:v1 .
```

### 3. Run the container
```bash
docker run -d --name devops-web-container -p 8080:80 devops-web:v1
```

### 4. Verify the running container
```bash
docker ps
```

### 5. Access the application
Open http://localhost:8080 in a browser.

### 6. List Docker images
```bash
docker images
```

## Result
The Docker image was built successfully, and the containerized web application was accessed through the browser.
