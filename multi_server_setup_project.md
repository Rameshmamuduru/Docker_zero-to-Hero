Perfect 👍
Below is a **CLEAR, CLEAN, REAL-TIME PRODUCTION-GRADE DOCUMENT**.

This is **not lab-style**, not mixed, not confusing.
You can **copy–paste**, **implement**, and **explain in interviews**.

---

# 📘 REAL-TIME PROD DEVOPS SETUP DOCUMENT

**(Multi-Server, Industry Standard)**

---

## 🧱 ARCHITECTURE OVERVIEW

```
Developer
   |
   v
GitHub
   |
   v
Jenkins Controller
   |
   +--> Jenkins Build Agent
   |        |
   |        +--> Trivy Scan
   |        +--> Docker Build
   |        +--> Docker Scout
   |
   +--> SonarQube Server
   |
   v
Docker Registry (DockerHub / ECR)
   |
   v
Application Server (Docker / K8s)
```

---

# 🖥 SERVER LIST (MINIMUM REQUIRED)

| Server   | Purpose             |
| -------- | ------------------- |
| Server-1 | Jenkins Controller  |
| Server-2 | Jenkins Build Agent |
| Server-3 | SonarQube Server    |
| Server-4 | Application Server  |

---

# 🔹 SERVER-1: JENKINS CONTROLLER SETUP

### Purpose

* Orchestrates pipelines
* Stores jobs, credentials
* **Does NOT build Docker images**

---

### Step-1: OS Prep

```bash
sudo -i
apt update && apt upgrade -y
```

### Step-2: Install Java

```bash
apt install openjdk-17-jre-headless -y
```

### Step-3: Install Jenkins

```bash
wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ \
> /etc/apt/sources.list.d/jenkins.list

apt update
apt install jenkins -y
```

### Step-4: Start Jenkins

```bash
systemctl enable jenkins
systemctl start jenkins
```

### Access

```
http://<JENKINS-IP>:8080
```

---

# 🔹 SERVER-2: JENKINS BUILD AGENT (IMPORTANT)

### Purpose

* Docker builds
* Trivy scans
* Docker Scout scans

---

### Step-1: OS Prep

```bash
sudo -i
apt update && apt upgrade -y
```

### Step-2: Install Docker

```bash
apt install -y ca-certificates curl

mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
-o /etc/apt/keyrings/docker.asc

echo "deb [signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" \
> /etc/apt/sources.list.d/docker.list

apt update
apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
systemctl enable docker
systemctl start docker
```

### Step-3: Install Trivy

```bash
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key \
| gpg --dearmor -o /usr/share/keyrings/trivy.gpg

echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] \
https://aquasecurity.github.io/trivy-repo/deb generic main" \
> /etc/apt/sources.list.d/trivy.list

apt update
apt install trivy -y
```

### Step-4: Install Docker Scout

```bash
curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh \
| sh -s -- -b /usr/local/bin
```

### Step-5: Connect Agent to Jenkins

Install **agent.jar** from Jenkins UI → Nodes → New Node
Run:

```bash
java -jar agent.jar -url http://<JENKINS-IP>:8080 \
-secret <SECRET> \
-name docker-agent
```

---

# 🔹 SERVER-3: SONARQUBE SERVER

### Purpose

* Code quality
* Security rules
* Long-running service

---

### Step-1: Install Docker

*(Same Docker steps as above)*

### Step-2: Run SonarQube

```bash
docker volume create sonar_data
docker volume create sonar_logs
docker volume create sonar_extensions

docker run -d \
--name sonar \
-p 9000:9000 \
-v sonar_data:/opt/sonarqube/data \
-v sonar_logs:/opt/sonarqube/logs \
-v sonar_extensions:/opt/sonarqube/extensions \
sonarqube:lts-community
```

### Access

```
http://<SONAR-IP>:9000
Login: admin / admin
```

Create **Sonar Token** → Save it.

---

# 🔹 SERVER-4: APPLICATION SERVER

### Purpose

* Runs application containers
* **No Jenkins, no scans**

---

### Step-1: Install Docker

*(Same Docker steps)*

### Step-2: Clone Repo

```bash
git clone https://github.com/your-org/app.git
cd app
```

### Step-3: Docker Compose Deploy

```bash
docker compose up -d
```

---

# 🔹 JENKINS UI CONFIGURATION

### Plugins

Install:

* Git
* Pipeline
* Docker Pipeline
* SonarQube Scanner

---

### Tools

**Sonar Scanner**

```
Name: sonar-scanner
✔ Install automatically
```

---

### Credentials

| Type        | ID          |
| ----------- | ----------- |
| DockerHub   | docker      |
| Sonar Token | sonar-token |
| SSH Key     | deploy-key  |

---

### Sonar Config

```
Name: sonar-prod
URL: http://<SONAR-IP>:9000
Token: sonar-token
```

---

# 📄 PROD-GRADE JENKINSFILE

```groovy
pipeline {
  agent { label 'docker-agent' }

  environment {
    IMAGE = "repo/app:${BUILD_NUMBER}"
  }

  stages {

    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Trivy Scan') {
      steps { sh 'trivy fs .' }
    }

    stage('SonarQube') {
      steps {
        withSonarQubeEnv('sonar-prod') {
          sh 'sonar-scanner'
        }
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }

    stage('Scout Scan') {
      steps {
        sh 'docker-scout cves $IMAGE'
      }
    }

    stage('Push Image') {
      steps {
        withDockerRegistry(credentialsId: 'docker') {
          sh 'docker push $IMAGE'
        }
      }
    }

    stage('Deploy') {
      steps {
        sshagent(['deploy-key']) {
          sh 'ssh ubuntu@APP-IP docker compose up -d'
        }
      }
    }
  }
}
```

---

# 🎯 WHY THIS IS REAL-TIME PROD

✔ Isolation
✔ Scalability
✔ Security
✔ Fault tolerance
✔ Interview-ready

---

## 📌 NEXT OPTIONS

* 📄 Convert this into **PDF**
* ☸ Kubernetes version
* ☁ AWS / Azure architecture
* 🧠 Interview Q&A

Just say **YES** 👍
