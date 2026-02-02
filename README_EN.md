# Jenkins CI/CD Pipeline - PayMyBuddy with Terraform & Ansible

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.7-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Docker](https://img.shields.io/badge/Docker-Enabled-blue.svg)](https://www.docker.com/)
[![Jenkins](https://img.shields.io/badge/CI%2FCD-Jenkins-red.svg)](https://www.jenkins.io/)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-purple.svg)](https://www.terraform.io/)
[![SonarCloud](https://img.shields.io/badge/Quality-SonarCloud-yellow.svg)](https://sonarcloud.io/)

Complete project demonstrating an **end-to-end CI/CD pipeline** implementation for a Spring Boot application, using Jenkins, Terraform, Docker, SonarCloud, and automated AWS deployment with Infrastructure as Code.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Jenkins Pipeline Setup](#jenkins-pipeline-setup)
- [Local Deployment](#local-deployment)
- [Detailed CI/CD Pipeline](#detailed-cicd-pipeline)
- [Environment Variables](#environment-variables)
- [Troubleshooting](#troubleshooting)
- [Contributors](#contributors)

## 🎯 Project Overview

**PayMyBuddy** is a web-based money transfer application between friends, developed with Spring Boot. This project demonstrates a complete **modern DevOps pipeline** implementation with:

### Continuous Integration and Deployment Pipeline

This project implements a **complete CI/CD pipeline** that automates:

- ✅ **Build and Tests**: Maven compilation, automated unit tests
- ✅ **Code Quality**: SonarCloud analysis with Quality Gate
- ✅ **Containerization**: Docker image build and push to DockerHub
- ✅ **Infrastructure as Code**: AWS provisioning with Terraform (EC2, Security Groups, Elastic IP)
- ✅ **Multi-Environment Deployment**: Automated staging and production deployments
- ✅ **Validation Testing**: Automated post-deployment tests
- ✅ **Infrastructure Management**: On-demand environment creation and destruction

### Business Application

Financial transaction management application between users featuring:
- Secure login and registration (Spring Security)
- User profile management
- Connection management (buddies/friends)
- Money transfers between users
- Bank account management
- Transaction history

## ✨ Features

### DevOps
- 🚀 Complete Jenkins pipeline with 13 automated stages
- 🏗️ Infrastructure as Code with Terraform (reusable modules)
- 🐳 Multi-service Docker containerization (App + MySQL)
- ☁️ Automated deployment on AWS EC2
- 📊 Quality analysis with SonarCloud
- 🔄 Separate staging and production deployments
- 🔒 Secure credential and secret management
- 🧪 Automated testing at each stage

### Application
- 👥 Complete user management
- 💸 Secure money transfers
- 🏦 Bank account integration
- 📱 Responsive web interface (Thymeleaf)
- 🔐 Authentication and authorization (Spring Security)
- 💾 Data persistence (MySQL + JPA)

## 🏗️ Architecture

![CI/CD Architecture](<CICD Jenkins.png>)

### Pipeline Flow

```
┌──────────────────────────────────────────────────────────────┐
│                     GIT PUSH (origin/iac)                    │
└──────────────────────┬───────────────────────────────────────┘
                       │
                       v
┌──────────────────────────────────────────────────────────────┐
│               JENKINS PIPELINE TRIGGER                       │
└──────────────────────┬───────────────────────────────────────┘
                       │
    ┌──────────────────┴──────────────────┐
    │                                     │
    v                                     v
┌─────────────────────┐         ┌─────────────────────┐
│  1. Maven Tests     │         │  2. SonarCloud      │
│  + JUnit Reports    │         │     Analysis        │
└─────────┬───────────┘         └──────────┬──────────┘
          │                                │
          v                                v
    ┌─────────────────────────────────────────┐
    │     3. Quality Gate (60s timeout)       │
    └─────────────────┬───────────────────────┘
                      │
                      v
    ┌─────────────────────────────────────────┐
    │  4. Package JAR + Build Docker Image    │
    │     5. Push to DockerHub                │
    └─────────────────┬───────────────────────┘
                      │
    ┌─────────────────┴─────────────────┐
    │                                   │
    v                                   v
┌──────────────────┐          ┌──────────────────┐
│   STAGING ENV    │          │  PRODUCTION ENV  │
├──────────────────┤          ├──────────────────┤
│ 6. Terraform     │          │ 10. Terraform    │
│    EC2 Staging   │          │     EC2 Prod     │
│ 7. Deploy SSH    │          │ 11. Deploy SSH   │
│ 8. Test Staging  │          │ 12. Test Prod    │
│ 9. Destroy (opt) │          │ 13. Destroy (opt)│
└──────────────────┘          └──────────────────┘
```

## 🛠️ Technology Stack

### Backend & Framework
- **Java 17** - Programming language
- **Spring Boot 2.7.1** - Application framework
- **Spring Security** - Authentication and authorization
- **Spring Data JPA** - Persistence layer
- **Thymeleaf** - Template engine
- **MySQL 8.0** - Relational database
- **Maven** - Dependency management and build

### DevOps & CI/CD
- **Jenkins** - CI/CD pipeline orchestration
- **Terraform** - Infrastructure as Code (AWS)
- **Ansible** - Configuration management
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **SonarCloud** - Code quality analysis
- **Git** - Version control

### Cloud & Infrastructure
- **AWS EC2** - Compute instances
- **AWS Security Groups** - Firewall
- **AWS Elastic IP** - Static IP addresses
- **AWS S3** - Terraform state storage
- **Amazon Corretto 17** - JDK distribution

### Registries & Repositories
- **DockerHub** - Docker image registry
- **GitHub** - Source code hosting

## 📂 Project Structure

```
jenkins-CICD-spring-boot-app/
│
├── src/                                    # Application source code
│   ├── main/
│   │   ├── java/com/paymybuddy/          # Java code
│   │   │   ├── config/                    # Spring configuration
│   │   │   ├── controller/                # MVC controllers
│   │   │   ├── model/                     # JPA entities
│   │   │   ├── repository/                # Repositories
│   │   │   ├── service/                   # Business logic
│   │   │   └── exceptions/                # Custom exceptions
│   │   └── resources/
│   │       ├── templates/                 # Thymeleaf templates
│   │       ├── static/                    # Static resources
│   │       └── database/                  # SQL scripts
│   └── test/                              # Unit and integration tests
│
├── iac/                                    # Infrastructure as Code (Terraform)
│   ├── modules/
│   │   └── ec2module/                     # Reusable EC2 module
│   │       ├── main.tf                    # AWS resources (EC2, SG, EIP)
│   │       └── variables.tf               # Module variables
│   ├── staging/                           # Staging environment
│   │   └── maint.tf                       # Terraform staging config
│   └── prod/                              # Production environment
│       └── main.tf                        # Terraform production config
│
├── deploy/                                 # Deployment configuration
│   ├── docker-compose.yml                 # Multi-container orchestration
│   ├── env/                               # Environment variables
│   │   ├── mysql.env                      # MySQL config
│   │   └── paymybuddy.env                 # Application config
│   ├── secrets/                           # Secrets (gitignored)
│   │   ├── db_user.txt                    # DB username
│   │   └── db_password.txt                # DB password
│   └── initdb/                            # DB initialization scripts
│       └── create.sql                     # Database schema
│
├── agent/                                  # Custom Jenkins agent
│   └── Dockerfile                         # Docker+Maven+Git image
│
├── Jenkinsfile                             # Jenkins CI/CD pipeline
├── Dockerfile                              # Application Docker image
├── pom.xml                                 # Maven configuration
├── mvnw / mvnw.cmd                         # Maven Wrapper
└── README.md                               # Documentation

```

## 📦 Prerequisites

### For Local Development
- **Java 17** or higher
- **Maven 3.8+** for building
- **MySQL 8.0+** for database
- **Git** for versioning
- **Docker Desktop** (optional but recommended)

### For CI/CD Pipeline
- **Jenkins Account** configured and accessible
- **AWS Account** with programmatic access (Access Key/Secret Key)
- **DockerHub Account** for image storage
- **SonarCloud Account** for code analysis
- **Terraform** installed (for local IaC testing)
- **SSH Key** for EC2 server access

### Recommended Versions
| Tool | Minimum Version | Tested Version |
|------|----------------|----------------|
| Java | 17 | 17 |
| Maven | 3.8 | 3.9.5 |
| Docker | 20.10 | 24.0 |
| Docker Compose | 2.0 | 2.23 |
| Terraform | 1.0 | 1.6.0 |
| Jenkins | 2.400 | 2.426 |
| MySQL | 8.0 | 8.0.35 |

## 🚀 Installation and Setup

### 1. Clone the Project

```bash
git clone https://github.com/AnselmeG300/jenkins-CICD-spring-boot-app.git
cd jenkins-CICD-spring-boot-app
```

### 2. MySQL Database Configuration

#### Install MySQL (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
```

#### Create database
```bash
sudo mysql -u root -p
```

```sql
CREATE DATABASE db_paymybuddy;
CREATE USER 'paymybuddy_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON db_paymybuddy.* TO 'paymybuddy_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### Initialize schema
```bash
mysql -u paymybuddy_user -p db_paymybuddy < src/main/resources/database/create.sql
```

### 3. Application Configuration

Create an `application-local.properties` file in `src/main/resources/`:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/db_paymybuddy
spring.datasource.username=paymybuddy_user
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# Server Configuration
server.port=8080
```

## 🔧 Local Deployment

### Option 1: Run with Maven

```bash
# Build the project
mvn clean install

# Start the application
mvn spring-boot:run -Dspring-boot.run.profiles=local
```

Access the application: **http://localhost:8080**

### Option 2: Run with JAR

```bash
# Compile and package
mvn clean package -DskipTests

# Execute JAR
java -jar target/paymybuddy.jar --spring.profiles.active=local
```

### Option 3: Deploy with Docker

#### Build Docker image
```bash
# Build Maven application
mvn clean package -DskipTests

# Build Docker image
docker build -t paymybuddy:latest .
```

#### Launch with Docker Compose

```bash
cd deploy
docker-compose up -d
```

This will start:
- ✅ MySQL container (port 3306)
- ✅ Application container (port 8080)
- ✅ Private bridge network
- ✅ Persistent volume for MySQL

#### Verify deployment
```bash
# Check containers
docker ps

# View logs
docker-compose logs -f paymybuddy

# Test application
curl http://localhost:8080
```

### Option 4: Manual Docker Deployment

```bash
# Create network
docker network create paymybuddy-network

# Start MySQL
docker run -d \
  --name paymybuddy-mysql \
  --network paymybuddy-network \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MYSQL_DATABASE=db_paymybuddy \
  -p 3306:3306 \
  mysql:8.0

# Wait for MySQL to be ready
sleep 30

# Start application
docker run -d \
  --name paymybuddy-app \
  --network paymybuddy-network \
  -p 8080:8080 \
  -e SPRING_DATASOURCE_URL=jdbc:mysql://paymybuddy-mysql:3306/db_paymybuddy \
  -e SPRING_DATASOURCE_USERNAME=root \
  -e SPRING_DATASOURCE_PASSWORD=password \
  paymybuddy:latest
```

## 🔨 Jenkins Pipeline Setup

### Step 1: Jenkins Installation

#### Installation on Ubuntu
```bash
# Install Java 17
sudo apt update
sudo apt install openjdk-17-jdk -y

# Add Jenkins repository
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt update
sudo apt install jenkins -y

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Get initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Access Jenkins: **http://your-server:8080**

### Step 2: Jenkins Plugin Installation

In **Manage Jenkins > Plugins**, install:
- ✅ **Docker Pipeline**
- ✅ **Git Plugin**
- ✅ **Maven Integration**
- ✅ **SonarQube Scanner**
- ✅ **SSH Agent Plugin**
- ✅ **AWS Credentials Plugin**
- ✅ **Pipeline Plugin**
- ✅ **JUnit Plugin**

### Step 3: Jenkins Credentials Configuration

#### 1. DockerHub Credentials
- Go to **Manage Jenkins > Credentials > System > Global credentials**
- Click **Add Credentials**
- Type: **Username with password**
- ID: `DockerHubCredentials`
- Username: your_dockerhub_username
- Password: your_dockerhub_token

#### 2. AWS Credentials
- Type: **AWS Credentials**
- ID: `AwsCredentials`
- Access Key ID: your_aws_access_key
- Secret Access Key: your_aws_secret_key

#### 3. SSH Credentials
- Type: **SSH Username with private key**
- ID: `SSH_AUTH_SERVER`
- Username: `centos` (or your EC2 user)
- Private Key: paste your SSH private key

#### 4. MySQL Credentials
- Type: **Username with password**
- ID: `MYSQL_AUTH`
- Username: root (or your MySQL user)
- Password: your_mysql_password

### Step 4: SonarCloud Configuration

#### 1. Create SonarCloud account
- Go to https://sonarcloud.io
- Login with GitHub
- Create a new organization and project

#### 2. Generate SonarCloud token
- In SonarCloud: **My Account > Security > Generate Token**

#### 3. Configure SonarCloud in Jenkins
- **Manage Jenkins > Configure System > SonarQube servers**
- Name: `SonarCloudServer`
- Server URL: `https://sonarcloud.io`
- Server authentication token: add generated token

#### 4. Create `.m2/settings.xml` file
```xml
<settings>
  <pluginGroups>
    <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
  </pluginGroups>
  <profiles>
    <profile>
      <id>sonar</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <sonar.host.url>https://sonarcloud.io</sonar.host.url>
        <sonar.organization>your-organization</sonar.organization>
        <sonar.projectKey>your-project-key</sonar.projectKey>
      </properties>
    </profile>
  </profiles>
</settings>
```

### Step 5: Terraform & AWS Configuration

#### 1. Create S3 bucket for Terraform state
```bash
aws s3 mb s3://terraform-jenkins-cicd --region us-east-1
```

#### 2. Create EC2 key pair
```bash
# In AWS EC2 Console > Key Pairs
# Create key named "devops-cicd-jenkins"
# Download .pem file
```

#### 3. Create Elastic IPs
```bash
# In AWS EC2 > Elastic IPs
# Allocate 2 new IP addresses
# Tag: "staging-jenkins" and "prod-jenkins"
```

### Step 6: Create Jenkins Pipeline

1. **Create new Pipeline job**
   - Click **New Item**
   - Name: `PayMyBuddy-CICD`
   - Type: **Pipeline**

2. **Configure Pipeline**
   - In **Pipeline > Definition**: choose **Pipeline script from SCM**
   - SCM: **Git**
   - Repository URL: `https://github.com/AnselmeG300/jenkins-CICD-spring-boot-app.git`
   - Branch: `*/iac`
   - Script Path: `Jenkinsfile`

3. **Configure Triggers** (optional)
   - Check **GitHub hook trigger for GITScm polling**
   - Or **Poll SCM**: `H/5 * * * *` (every 5 minutes)

4. **Save and Run**
   - Click **Save**
   - Click **Build Now**

## 🔄 Detailed CI/CD Pipeline

The Jenkins pipeline consists of **13 automated stages** that execute based on the Git branch.

### Stage Overview

| Stage | Name | Description | Approx. Duration |
|-------|------|-------------|------------------|
| 1 | **Test** | Run Maven unit tests | 2-3 min |
| 2 | **SonarCloud Analysis** | Code quality analysis | 1-2 min |
| 3 | **Quality Gate** | Validate SonarCloud thresholds | 60 sec |
| 4 | **Package** | Build Maven JAR | 1-2 min |
| 5 | **Build & Push Image** | Docker build + push to DockerHub | 2-3 min |
| 6 | **IAC Staging** | Terraform apply staging environment | 2-3 min |
| 7 | **Deploy Staging** | SSH deployment to staging EC2 | 1-2 min |
| 8 | **Test Staging** | Staging validation tests | 30 sec |
| 9 | **Destroy Staging** | Destroy staging infra (manual) | 2 min |
| 10 | **IAC Prod** | Terraform apply production environment | 2-3 min |
| 11 | **Deploy Prod** | SSH deployment to production EC2 | 1-2 min |
| 12 | **Test Prod** | Production validation tests | 30 sec |
| 13 | **Destroy Prod** | Destroy production infra (manual) | 2 min |

**Total estimated duration**: ~20-30 minutes (complete pipeline)

### Stage Details

#### 🧪 Stage 1: Automated Tests
```groovy
stage('Test') {
    steps {
        sh 'mvn clean test'
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
```
- Runs all unit tests
- Generates JUnit reports
- Publishes results in Jenkins

#### 📊 Stage 2: SonarCloud Analysis
```groovy
stage('SonarCloud analysis') {
    steps {
        withSonarQubeEnv('SonarCloudServer') {
            sh 'mvn sonar:sonar -s .m2/settings.xml'
        }
    }
}
```
- Analyzes code quality
- Detects bugs, vulnerabilities, code smells
- Calculates code coverage

#### ✅ Stage 3: Quality Gate
```groovy
stage('Quality Gate') {
    steps {
        timeout(time: 60, unit: 'SECONDS') {
            waitForQualityGate abortPipeline: false
        }
    }
}
```
- Waits for SonarCloud validation
- 60-second timeout
- Does not abort pipeline on failure

#### 📦 Stage 4: Package
```groovy
stage('Package') {
    steps {
        sh 'mvn clean package -DskipTests'
    }
}
```
- Compiles source code
- Creates executable JAR file
- Skips tests (already executed)

#### 🐳 Stage 5: Build and Push Docker
```groovy
stage('Build and push IMAGE to docker registry') {
    steps {
        sh """
            docker build -t ${DOCKERHUB_AUTH_USR}/${IMAGE_NAME}:${IMAGE_TAG} .
            echo ${DOCKERHUB_AUTH_PSW} | docker login -u ${DOCKERHUB_AUTH_USR} --password-stdin
            docker push ${DOCKERHUB_AUTH_USR}/${IMAGE_NAME}:${IMAGE_TAG}
        """
    }
}
```
- Builds Docker application image
- Logs into DockerHub
- Pushes image to registry

#### ☁️ Stage 6: Staging Infrastructure (Terraform)
```groovy
stage('IAC Staging on aws') {
    when {
        expression { GIT_BRANCH == 'origin/iac' }
    }
    agent {
        docker {
            image 'jenkins/jnlp-agent-terraform'
        }
    }
    steps {
        withCredentials([aws(...)]) {
            dir('iac/staging') {
                sh '''
                    terraform init
                    terraform plan
                    terraform apply -auto-approve
                    sleep 60
                '''
            }
        }
    }
}
```
- Executes only on `iac` branch
- Provisions EC2 instance on AWS
- Configures Security Groups, Elastic IP
- Waits 60 seconds for initialization

#### 🚀 Stage 7: Staging Deployment
```groovy
stage('Deploy in staging') {
    steps {
        sshagent(credentials: ['SSH_AUTH_SERVER']) {
            sh '''
                ssh-keyscan -t rsa,dsa ${HOSTNAME_DEPLOY_STAGING} >> ~/.ssh/known_hosts
                scp -r deploy centos@${HOSTNAME_DEPLOY_STAGING}:/home/centos/
                [Deployment commands...]
            '''
        }
    }
}
```
- Transfers deployment files via SCP
- Configures environment variables
- Launches Docker Compose on remote server
- Starts application and MySQL

#### 🧪 Stage 8: Staging Tests
```groovy
stage('Test Staging') {
    steps {
        sh '''
            sleep 30
            apk add --no-cache curl
            curl ${HOSTNAME_DEPLOY_STAGING}:8080
        '''
    }
}
```
- Waits 30 seconds for startup
- Tests application availability
- Verifies HTTP response

#### 🗑️ Stage 9: Staging Destruction (Optional)
```groovy
stage('Destroy staging') {
    steps {
        timeout(time: 10, unit: "MINUTES") {
            input message: "Confirm staging deletion?", ok: 'Yes'
        }
        withCredentials([aws(...)]) {
            sh 'terraform destroy -auto-approve'
        }
    }
}
```
- Requests manual confirmation
- 10-minute timeout
- Destroys all staging infrastructure

#### 📋 Stages 10-13: Identical for Production
Stages 10-13 repeat the same process for the **production** environment:
- IAC Prod → Deploy Prod → Test Prod → Destroy Prod

### Pipeline Environment Variables

```groovy
environment {
    DOCKERHUB_AUTH = credentials('DockerHubCredentials')
    MYSQL_AUTH = credentials('MYSQL_AUTH')
    HOSTNAME_DEPLOY_PROD = "34.197.213.138"
    HOSTNAME_DEPLOY_STAGING = "34.233.177.253"
    IMAGE_NAME = 'paymybuddy'
    IMAGE_TAG = 'latest'
}
```

### Jenkins Agents Used

#### 1. Custom Docker agent (Build & Deploy)
```dockerfile
FROM docker:20
RUN apk add --no-cache git
RUN apk add --no-cache maven
```

#### 2. Terraform agent (IaC)
```groovy
agent {
    docker {
        image 'jenkins/jnlp-agent-terraform'
    }
}
```

## 🔐 Environment Variables

### Application (Spring Boot)

| Variable | Description | Default Value | Required |
|----------|-------------|---------------|----------|
| `SPRING_DATASOURCE_URL` | MySQL connection URL | `jdbc:mysql://localhost:3306/db_paymybuddy` | ✅ |
| `SPRING_DATASOURCE_USERNAME` | Database username | `root` | ✅ |
| `SPRING_DATASOURCE_PASSWORD` | Database password | - | ✅ |
| `SERVER_PORT` | Web server port | `8080` | ❌ |
| `SPRING_JPA_HIBERNATE_DDL_AUTO` | Schema management mode | `update` | ❌ |

### Docker Compose

| Variable | Description | File |
|----------|-------------|------|
| `IMAGE_VERSION` | Docker image version | `.env` |
| `MYSQL_ROOT_PASSWORD` | MySQL root password | `env/mysql.env` |
| `MYSQL_DATABASE` | Database name | `env/mysql.env` |

### Terraform

| Variable | Description | Value |
|----------|-------------|-------|
| `instance_type` | EC2 instance type | `t2.medium` |
| `aws_region` | AWS region | `us-east-1` |
| `sg_name` | Security Group name | `cicd-prod-sg` |
| `eip_name` | Elastic IP name | `prod-jenkins` |

### Jenkins Credentials (to configure)

| Credential ID | Type | Usage |
|---------------|------|-------|
| `DockerHubCredentials` | Username/Password | Push Docker images |
| `AwsCredentials` | AWS Credentials | Terraform AWS |
| `SSH_AUTH_SERVER` | SSH Private Key | EC2 deployment |
| `MYSQL_AUTH` | Username/Password | MySQL database |

## 🐛 Troubleshooting

### Common Issues

#### 1. MySQL Connection Error

**Problem**: `CommunicationsException: Communications link failure`

**Solutions**:
```bash
# Check MySQL is running
docker ps | grep mysql
# or
sudo systemctl status mysql

# Test connection
mysql -u root -p -h localhost

# Check MySQL logs
docker logs paymybuddydb

# Verify Docker network
docker network inspect paymybuddynetwork
```

#### 2. Port 8080 Already in Use

**Problem**: `Bind for 0.0.0.0:8080 failed: port is already allocated`

**Solutions**:
```bash
# Windows - Find process
netstat -ano | findstr :8080
taskkill /PID <PID> /F

# Linux - Find process
lsof -i :8080
kill -9 <PID>

# Or use different port
docker run -p 8081:8080 paymybuddy:latest
```

#### 3. Container Stops Immediately

**Problem**: Container starts then stops

**Solutions**:
```bash
# View logs
docker logs paymybuddy

# Check environment variables
docker inspect paymybuddy | grep -A 20 "Env"

# Run in interactive mode for debugging
docker run -it paymybuddy:latest /bin/sh
```

#### 4. SonarCloud Quality Gate Failure

**Problem**: Pipeline fails at Quality Gate stage

**Solutions**:
- Verify SonarCloud configuration in `.m2/settings.xml`
- Check SonarCloud token is valid
- Review SonarCloud dashboard for details
- Adjust quality thresholds if necessary

#### 5. Terraform "State Lock" Error

**Problem**: `Error acquiring the state lock`

**Solutions**:
```bash
# Force unlock (CAUTION)
terraform force-unlock <LOCK_ID>

# Check state in S3
aws s3 ls s3://terraform-jenkins-cicd/
```

#### 6. SSH Deployment Failure

**Problem**: Cannot connect to EC2

**Solutions**:
```bash
# Check instance is running
aws ec2 describe-instances --instance-ids <instance-id>

# Test SSH connection manually
ssh -i devops-cicd-jenkins.pem centos@<IP>

# Verify Security Groups
# Port 22 must be open for Jenkins IP
```

#### 7. Docker Image Not Found

**Problem**: `Error: image not found`

**Solutions**:
```bash
# Check image exists
docker images | grep paymybuddy

# Rebuild image
mvn clean package -DskipTests
docker build -t paymybuddy:latest .

# Pull from DockerHub
docker pull <dockerhub-user>/paymybuddy:latest
```

### Useful Diagnostic Commands

```bash
# === Docker ===
docker ps -a                              # All containers
docker logs -f <container>                # Real-time logs
docker inspect <container>                # Container details
docker exec -it <container> /bin/sh       # Enter container
docker stats                              # Resource usage

# === Docker Compose ===
docker-compose logs -f                    # All service logs
docker-compose ps                         # Service status
docker-compose down -v                    # Stop and clean

# === MySQL ===
docker exec -it paymybuddydb mysql -u root -p    # MySQL access
SHOW DATABASES;                           # List databases
USE db_paymybuddy;                        # Use database
SHOW TABLES;                              # List tables

# === Application ===
curl http://localhost:8080                # HTTP test
curl http://localhost:8080/actuator/health  # Health check
netstat -tuln | grep 8080                 # Check port

# === Jenkins ===
# View Jenkins logs
sudo journalctl -u jenkins -f

# Restart Jenkins
sudo systemctl restart jenkins

# === Terraform ===
terraform state list                      # List resources
terraform show                            # Display state
terraform output                          # View outputs
terraform refresh                         # Refresh state
```

### Complete Cleanup

```bash
# Stop all containers
docker stop $(docker ps -aq)

# Remove all containers
docker rm $(docker ps -aq)

# Remove PayMyBuddy images
docker rmi $(docker images | grep paymybuddy | awk '{print $3}')

# Complete Docker cleanup
docker system prune -a --volumes

# Reset database
docker volume rm deploy_db_paymybuddy
```

## 📚 Additional Resources

### Official Documentation
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Docker Documentation](https://docs.docker.com/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [SonarCloud Documentation](https://docs.sonarcloud.io/)

### Recommended Tutorials
- [Spring Boot with MySQL](https://spring.io/guides/gs/accessing-data-mysql/)
- [Jenkins Pipeline Tutorial](https://www.jenkins.io/doc/book/pipeline/)
- [Terraform AWS EC2](https://learn.hashicorp.com/tutorials/terraform/aws-build)
- [Docker Multi-Container Apps](https://docs.docker.com/compose/)

### Monitoring Tools (Optional)
- **Prometheus**: Metrics and monitoring
- **Grafana**: Visualization dashboards
- **ELK Stack**: Log centralization
- **Datadog**: Cloud monitoring

## 🤝 Contributing

Contributions are welcome! Here's how to participate:

1. **Fork** the project
2. **Create** a feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit** your changes
   ```bash
   git commit -m "Add amazing feature"
   ```
4. **Push** to the branch
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open** a Pull Request

### Contribution Guidelines
- ✅ Write tests for new features
- ✅ Follow existing code style
- ✅ Document changes in README
- ✅ Ensure Jenkins pipeline passes

## 📄 License

This project is licensed under the **MIT License**. See `LICENSE` file for details.

## 👨‍💻 Author

**AnselmeG300**

- GitHub: [@AnselmeG300](https://github.com/AnselmeG300)
- Email: [contact@paymybuddy.com](mailto:contact@paymybuddy.com)

## 🙏 Acknowledgments

- **Spring Boot Team** for the exceptional framework
- **HashiCorp** for Terraform
- **Jenkins Community** for CI/CD tools
- **Docker Inc.** for containerization platform
- **SonarCloud** for code quality analysis
- **AWS** for cloud infrastructure

---

## 📞 Support

For questions or issues:

- 📧 **Email**: support@paymybuddy.com
- 🐛 **Issues**: [GitHub Issues](https://github.com/AnselmeG300/jenkins-CICD-spring-boot-app/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/AnselmeG300/jenkins-CICD-spring-boot-app/discussions)

---

**⭐ If you find this project useful, please give it a star on GitHub!**

---

*Last updated: February 2026*
