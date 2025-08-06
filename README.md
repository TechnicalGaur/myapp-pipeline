# 🚀 Jenkins CI/CD Pipeline on AWS EC2 using Docker & GitHub

![Build](https://img.shields.io/badge/Build-Passing-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-AWS%20EC2-orange?style=flat-square)
![Tech](https://img.shields.io/badge/Tools-Jenkins%2C%20Docker%2C%20Node.js-critical?style=flat-square)

> A complete end-to-end **CI/CD pipeline** that builds, tests, and deploys a Node.js app using **Jenkins**, **Docker**, and **GitHub Webhooks**, hosted on **AWS EC2**.

---

## 📑 Table of Contents

- [🧰 Tech Stack](#-tech-stack)
- [📂 Project Structure](#-project-structure)
- [🚀 Features](#-features)
- [🔧 Setup Guide](#-setup-guide)
- [📄 Jenkinsfile Explained](#-jenkinsfile-explained)
- [🌐 GitHub Webhook Setup](#-github-webhook-setup)
- [🧪 Sample App Details](#-sample-app-details)
- [📸 Screenshots](#-screenshots)
- [🙋‍♂️ Author](#-author)
- [📜 License](#-license)

---

## 🧰 Tech Stack

| Tool        | Purpose                                |
|-------------|----------------------------------------|
| **Jenkins** | Automate CI/CD workflow                |
| **Docker**  | Containerize and deploy app            |
| **Node.js** | Sample web application backend         |
| **GitHub**  | Version control and webhook triggers   |
| **AWS EC2** | Hosting Jenkins and deployed container |

---

## 📂 Project Structure

myapp-pipeline/
├── Dockerfile # Docker image definition
├── Jenkinsfile # CI/CD pipeline definition
├── index.js # Node.js sample application
├── package.json # Project metadata & dependencies
└── README.md # This documentation file


---

## 🚀 Features

- 🔄 **Automated Build** – Build Docker image with Jenkins on every Git push
- ✅ **Test Execution** – Run unit tests (if defined) during pipeline
- 📦 **Containerized Deployment** – Deploy app via Docker on EC2
- 🌐 **Live Preview** – App accessible at EC2’s public IP (port 3000)
- 📡 **Webhook Integration** – Trigger pipeline automatically via GitHub Webhook

---

## 🔧 Setup Guide

<details>
<summary>🖥️ EC2 Instance Setup</summary>

1. Launch **Ubuntu 22.04** EC2 instance.
2. Open ports `22`, `8080`, and `3000`.
3. SSH into the instance:
   ```bash
   ssh -i "key.pem" ubuntu@<EC2_PUBLIC_IP>


</details> <details> <summary>🐳 Install Docker</summary>

sudo apt update && sudo apt install docker.io -y
sudo systemctl start docker && sudo systemctl enable docker


</details> <details> <summary>🔧 Install Jenkins</summary>

sudo apt install openjdk-17-jdk -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update && sudo apt install jenkins -y
sudo systemctl start jenkins && sudo systemctl enable jenkins


Access Jenkins at http://<EC2_PUBLIC_IP>:8080 and finish setup.

</details> <details> <summary>⚙️ Docker Permission Fix</summary>


sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

</details>


📄 Jenkinsfile Explained


pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test || echo "No tests defined"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying Docker container...'
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d -p 3000:3000 --name myapp myapp:latest
                '''
            }
        }
    }
}

Build: Creates Docker image.

Test: Executes tests (if any).

Deploy: Runs the container on EC2.


🌐 GitHub Webhook Setup
Navigate to GitHub → Repo → Settings → Webhooks → Add Webhook

Payload URL:
http://<EC2_PUBLIC_IP>:8080/github-webhook/

Content type: application/json

Trigger on: Just the push event


📸 Screenshots

<img width="1324" height="722" alt="Screenshot 2025-08-06 164113" src="https://github.com/user-attachments/assets/953d1212-e236-4310-ac97-8e301eabdce5" />


<img width="1331" height="586" alt="Screenshot 2025-08-06 164215" src="https://github.com/user-attachments/assets/d060031c-0d00-41dd-929e-df7ad9259e43" />


<img width="1343" height="593" alt="Screenshot 2025-08-06 164248" src="https://github.com/user-attachments/assets/98cacab8-7116-4198-ae5c-3ed1eff08b35" />




📜 License

This project is licensed under the MIT License.
Feel free to use, modify, and share!






