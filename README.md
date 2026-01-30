# 📘 Django Notes App – CI/CD with Jenkins & Docker

A simple **Django-based Notes Application** containerized using **Docker** and deployed automatically using a **Jenkins CI/CD pipeline** triggered via **GitHub Webhooks**.

---

## 🚀 Features
- Notes management using Django
- Dockerized application
- Jenkins CI/CD pipeline
- Docker image pushed to Docker Hub
- Automatic build on GitHub commit (Webhook)
- Blue Ocean pipeline visualization

---

## 🛠️ Tech Stack
- **Backend:** Django (Python)
- **CI/CD:** Jenkins (Pipeline + Blue Ocean)
- **Containerization:** Docker & Docker Compose
- **Version Control:** Git & GitHub
- **OS:** Ubuntu Linux

---

## 📂 Project Structure
Django-notes-app/
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── manage.py
├── requirements.txt
├── notes/
├── templates/
└── README.md


---

## 🐳 Docker Setup

### Build Docker Image
```bash
docker build -t note-app-test-new .
Run Application
docker compose up -d
🔄 CI/CD Pipeline Flow (Jenkins)
Code Clone – Pulls source code from GitHub

Build and Test – Builds Docker image

Push to Docker Hub – Pushes image to Docker Hub

Deploy – Deploys app using Docker Compose

Pipeline is automatically triggered using GitHub Webhooks.

📜 Jenkinsfile
pipeline {
    agent { label 'onkar' }

    stages {

        stage('Code Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/onkar-1817/Django-notes-app.git'
            }
        }

        stage('Build and Test') {
            steps {
                sh 'docker build -t note-app-test-new .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHub',
                        usernameVariable: 'dockerHubUser',
                        passwordVariable: 'dockerHubPass'
                    )
                ]) {
                    sh '''
                    docker tag note-app-test-new ${dockerHubUser}/note-app-test-new:latest
                    docker login -u ${dockerHubUser} -p ${dockerHubPass}
                    docker push ${dockerHubUser}/note-app-test-new:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker compose down
                docker compose up -d
                '''
            }
        }
    }
}
🔔 GitHub Webhook Setup
Webhook URL:

http://<JENKINS-IP>:8080/github-webhook/
Trigger Event:

☑ Push events

📊 Blue Ocean
Visual pipeline view

Stage-wise execution

Easy debugging

▶️ Run Locally (Without Jenkins)
git clone https://github.com/onkar-1817/Django-notes-app.git
cd Django-notes-app
docker compose up -d
Open browser:

http://localhost:8000
👨‍💻 Author
Onkar Ghugare
DevOps | Jenkins | Docker | CI/CD

🔗 GitHub: https://github.com/onkar-1817


