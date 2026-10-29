# Experiment 4: Docker Image with Jenkins Pipeline

## Aim

To build a Docker image, push it to Docker Hub, and automate the process using a Jenkins Pipeline.

## Objectives

- To create a simple Python application.
- To containerize the application using Docker.
- To automate Docker image building using Jenkins Pipeline.
- To authenticate with Docker Hub using Jenkins credentials.
- To push the Docker image to Docker Hub.

## Software and Tools Required

- macOS
- Docker Desktop
- Jenkins
- Git and GitHub
- Docker Hub
- Visual Studio Code

## Project Structure

```text
DevOpsIA/
└── Experiment-4-Docker-Jenkins/
    ├── Jenkinsfile
    ├── README.md
    └── myapp/
        ├── app.py
        └── Dockerfile
```

## Procedure

### Step 1: Create the Python Application

Create `myapp/app.py` with the following code:

```python
print("Hello from Docker and Jenkins Pipeline!")
```

### Step 2: Create the Dockerfile

Create `myapp/Dockerfile`:

```dockerfile
FROM python:3.10
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]
```

**Explanation:**

- `FROM` specifies the Python base image.
- `WORKDIR` sets the working directory inside the container.
- `COPY` copies the Python application into the image.
- `CMD` executes the Python application when the container starts.

### Step 3: Build and Test the Docker Image

Navigate to the application directory:

```bash
cd Experiment-4-Docker-Jenkins/myapp
```

Build the image:

```bash
docker build -t myapp:latest .
```

Run the container:

```bash
docker run --rm myapp:latest
```

Expected output:

```text
Hello from Docker and Jenkins Pipeline!
```

### Step 4: Configure Jenkins Docker Access

Jenkins was run as a Docker container with persistent storage and access to the Docker socket.

The Docker CLI was installed inside the Jenkins container to allow pipeline stages to execute Docker commands.

Verify Docker access:

```bash
docker exec jenkins docker --version
docker exec jenkins docker info
```

### Step 5: Create the Jenkinsfile

Create `Experiment-4-Docker-Jenkins/Jenkinsfile` with the following pipeline:


pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ajithraj2525/myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AjithRaj2525/DevOpsIA.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('Experiment-4-Docker-Jenkins/myapp') {
                    sh 'docker build -t $DOCKER_IMAGE:exp10 .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_PAT'
                )]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PAT" | docker login \
                            -u ajithraj2525 --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:exp10'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}


### Step 6: Configure Docker Hub Credentials in Jenkins

1. Open Jenkins at `http://localhost:8080`.
2. Navigate to **Manage Jenkins → Credentials**.
3. Open the global credentials store.
4. Click **Add Credentials**.
5. Select **Secret text** as the credential kind.
6. Enter your Docker Hub personal access token as the secret.
7. Set the ID to `dockerhub-PAT`.
8. Save the credential.

The pipeline uses this credential to authenticate with Docker Hub without hardcoding the access token in the Jenkinsfile.

### Step 7: Create the Jenkins Pipeline Job

1. Open Jenkins.
2. Click **New Item**.
3. Enter `Experiment-4-Docker-Pipeline` as the job name.
4. Select **Pipeline** and click **OK**.
5. Under the Pipeline section, configure:

| Setting | Value |
|---|---|
| Definition | Pipeline script from SCM |
| SCM | Git |
| Repository URL | `https://github.com/AjithRaj2525/DevOpsIA.git` |
| Branch Specifier | `*/main` |
| Script Path | `Experiment-4-Docker-Jenkins/Jenkinsfile` |

6. Click **Save**.

### Step 8: Execute the Pipeline

Click **Build Now** in the Jenkins job.

The pipeline performs these stages:

1. **Clone Repository:** Clones the GitHub repository.
2. **Build Docker Image:** Builds the image from the Dockerfile.
3. **Login to Docker Hub:** Authenticates using Jenkins credentials.
4. **Push Docker Image:** Pushes the image to Docker Hub.

## Docker Image Details

| Property | Value |
|---|---|
| Docker Hub username | `ajithraj2525` |
| Image repository | `myapp` |
| Image tag | `exp10` |
| Full image name | `ajithraj2525/myapp:exp10` |

## Verification

Verify that the image is available locally:

```bash
docker exec jenkins docker image ls
```

Verify the published image by visiting Docker Hub and checking the `myapp` repository for the `exp10` tag.

The published image can also be pulled using:

```bash
docker pull ajithraj2525/myapp:exp10
```

## Result

The Docker image creation and Jenkins Pipeline automation were completed successfully. Jenkins cloned the GitHub repository, built the Docker image, authenticated with Docker Hub using stored credentials, and pushed the image to Docker Hub.

## Windows version of Jenkins


pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ajithraj2525/myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AjithRaj2525/DevOpsIA.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:exp10 Experiment-4-Docker-Jenkins\\myapp'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                        @echo off
                        echo %DOCKER_PAT%|docker login -u ajithraj2525 --password-stdin
                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:exp10'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
