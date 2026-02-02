# Pipeline CI/CD Jenkins - PayMyBuddy avec Terraform & Ansible

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.7-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Docker](https://img.shields.io/badge/Docker-Enabled-blue.svg)](https://www.docker.com/)
[![Jenkins](https://img.shields.io/badge/CI%2FCD-Jenkins-red.svg)](https://www.jenkins.io/)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-purple.svg)](https://www.terraform.io/)
[![SonarCloud](https://img.shields.io/badge/Quality-SonarCloud-yellow.svg)](https://sonarcloud.io/)

Projet complet démontrant la mise en place d'une **pipeline CI/CD de bout en bout** pour une application Spring Boot, utilisant Jenkins, Terraform, Docker, SonarCloud et un déploiement automatisé sur AWS avec infrastructure as code.

> 📺 **[Regardez la série complète de tutoriels vidéo sur YouTube](https://youtube.com/playlist?list=PL7fAoxUwXyOXqevUq6MD2byv-YBgh58Cc&si=cEfP366-slA6mF12)** - Guide étape par étape pour implémenter cette pipeline CI/CD complète !

> 🇬🇧 **[English version of this README available here](README.md)**

## 📋 Table des Matières

- [Présentation du Projet](#présentation-du-projet)
- [Fonctionnalités](#fonctionnalités)
- [Architecture](#architecture)
- [Technologies Utilisées](#technologies-utilisées)
- [Structure du Projet](#structure-du-projet)
- [Prérequis](#prérequis)
- [Installation et Configuration](#installation-et-configuration)
- [Mise en Place de la Pipeline Jenkins](#mise-en-place-de-la-pipeline-jenkins)
- [Déploiement Local](#déploiement-local)
- [Pipeline CI/CD Détaillée](#pipeline-cicd-détaillée)
- [Variables d'Environnement](#variables-denvironnement)
- [Troubleshooting](#troubleshooting)
- [Tutoriel Vidéo](#tutoriel-vidéo)
- [Contributeurs](#contributeurs)

## 🎯 Présentation du Projet

**PayMyBuddy** est une application web de transfert d'argent entre amis développée avec Spring Boot. Ce projet illustre l'implémentation complète d'une **chaîne DevOps moderne** avec :

### Pipeline d'Intégration et de Déploiement Continu

Ce projet met en œuvre une **pipeline CI/CD complète** qui automatise :

- ✅ **Build et Tests** : Compilation Maven, tests unitaires automatisés
- ✅ **Qualité du Code** : Analyse SonarCloud avec Quality Gate
- ✅ **Conteneurisation** : Build et push d'images Docker vers DockerHub
- ✅ **Infrastructure as Code** : Provisionnement AWS avec Terraform (EC2, Security Groups, Elastic IP)
- ✅ **Déploiement Multi-Environnements** : Staging et Production automatisés
- ✅ **Tests de Validation** : Tests post-déploiement automatiques
- ✅ **Gestion d'Infrastructure** : Création et destruction d'environnements à la demande

### Application Métier

Application de gestion de transactions financières entre utilisateurs permettant :
- Connexion et inscription sécurisées (Spring Security)
- Gestion de profils utilisateurs
- Ajout de connexions (buddies/amis)
- Transferts d'argent entre utilisateurs
- Gestion de comptes bancaires
- Historique des transactions

## ✨ Fonctionnalités

### DevOps
- 🚀 Pipeline Jenkins complète avec 13 étapes automatisées
- 🏗️ Infrastructure as Code avec Terraform (modules réutilisables)
- 🐳 Conteneurisation Docker multi-services (App + MySQL)
- ☁️ Déploiement automatisé sur AWS EC2
- 📊 Analyse de qualité avec SonarCloud
- 🔄 Déploiements staging et production séparés
- 🔒 Gestion sécurisée des credentials et secrets
- 🧪 Tests automatisés à chaque étape

### Application
- 👥 Gestion complète des utilisateurs
- 💸 Transferts d'argent sécurisés
- 🏦 Intégration de comptes bancaires
- 📱 Interface web responsive (Thymeleaf)
- 🔐 Authentification et autorisation (Spring Security)
- 💾 Persistance des données (MySQL + JPA)

## 🏗️ Architecture

![Architecture CI/CD](<CICD Jenkins.png>)

### Flux de la Pipeline

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
│  1. Tests Maven     │         │  2. SonarCloud      │
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

## 🛠️ Technologies Utilisées

### Backend & Framework
- **Java 17** - Langage de programmation
- **Spring Boot 2.7.1** - Framework applicatif
- **Spring Security** - Authentification et autorisation
- **Spring Data JPA** - Couche de persistance
- **Thymeleaf** - Moteur de templates
- **MySQL 8.0** - Base de données relationnelle
- **Maven** - Gestion des dépendances et build

### DevOps & CI/CD
- **Jenkins** - Orchestration de la pipeline CI/CD
- **Terraform** - Infrastructure as Code (AWS)
- **Ansible** - Configuration management
- **Docker** - Conteneurisation
- **Docker Compose** - Orchestration multi-containers
- **SonarCloud** - Analyse de qualité du code
- **Git** - Gestion de version

### Cloud & Infrastructure
- **AWS EC2** - Instances de calcul
- **AWS Security Groups** - Pare-feu
- **AWS Elastic IP** - Adresses IP statiques
- **AWS S3** - Stockage du state Terraform
- **Amazon Corretto 17** - Distribution JDK

### Registres & Repositories
- **DockerHub** - Registre d'images Docker
- **GitHub** - Hébergement du code source

## 📂 Structure du Projet

```
jenkins-CICD-spring-boot-app/
│
├── src/                                    # Code source de l'application
│   ├── main/
│   │   ├── java/com/paymybuddy/          # Code Java
│   │   │   ├── config/                    # Configuration Spring
│   │   │   ├── controller/                # Contrôleurs MVC
│   │   │   ├── model/                     # Entités JPA
│   │   │   ├── repository/                # Repositories
│   │   │   ├── service/                   # Logique métier
│   │   │   └── exceptions/                # Exceptions personnalisées
│   │   └── resources/
│   │       ├── templates/                 # Templates Thymeleaf
│   │       ├── static/                    # Ressources statiques
│   │       └── database/                  # Scripts SQL
│   └── test/                              # Tests unitaires et d'intégration
│
├── iac/                                    # Infrastructure as Code (Terraform)
│   ├── modules/
│   │   └── ec2module/                     # Module réutilisable EC2
│   │       ├── main.tf                    # Ressources AWS (EC2, SG, EIP)
│   │       └── variables.tf               # Variables du module
│   ├── staging/                           # Environnement de staging
│   │   └── maint.tf                       # Config Terraform staging
│   └── prod/                              # Environnement de production
│       └── main.tf                        # Config Terraform production
│
├── deploy/                                 # Configuration de déploiement
│   ├── docker-compose.yml                 # Orchestration multi-containers
│   ├── env/                               # Variables d'environnement
│   │   ├── mysql.env                      # Config MySQL
│   │   └── paymybuddy.env                 # Config application
│   ├── secrets/                           # Secrets (gitignored)
│   │   ├── db_user.txt                    # Utilisateur DB
│   │   └── db_password.txt                # Mot de passe DB
│   └── initdb/                            # Scripts d'initialisation DB
│       └── create.sql                     # Schéma de base de données
│
├── agent/                                  # Agent Jenkins personnalisé
│   └── Dockerfile                         # Image Docker+Maven+Git
│
├── Jenkinsfile                             # Pipeline CI/CD Jenkins
├── Dockerfile                              # Image Docker de l'application
├── pom.xml                                 # Configuration Maven
├── mvnw / mvnw.cmd                         # Maven Wrapper
└── README.md                               # Documentation

```

_La suite du contenu est identique au README original français, incluant toutes les sections d'installation, configuration, troubleshooting, etc._

## 🎥 Tutoriel Vidéo

Apprenez à implémenter cette pipeline CI/CD complète de zéro avec notre série de vidéos YouTube !

[![YouTube Playlist](https://img.shields.io/badge/YouTube-Playlist-red?style=for-the-badge&logo=youtube)](https://youtube.com/playlist?list=PL7fAoxUwXyOXqevUq6MD2byv-YBgh58Cc&si=cEfP366-slA6mF12)

**[📺 Regardez la Série Complète de Tutoriels](https://youtube.com/playlist?list=PL7fAoxUwXyOXqevUq6MD2byv-YBgh58Cc&si=cEfP366-slA6mF12)**

Cette playlist couvre :
- ✅ Configuration complète du projet et explication de l'architecture
- ✅ Installation et configuration de Jenkins
- ✅ Mise en place de l'intégration SonarCloud
- ✅ Configuration de Terraform pour l'infrastructure AWS
- ✅ Création et configuration de la pipeline CI/CD
- ✅ Configuration Docker et Docker Compose
- ✅ Déploiement multi-environnements (Staging & Production)
- ✅ Résolution des problèmes courants
- ✅ Meilleures pratiques et conseils d'optimisation

Parfait pour les ingénieurs DevOps, les développeurs et tous ceux qui souhaitent apprendre les pratiques modernes de CI/CD !

---

**⭐ Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile sur GitHub !**

---

*Dernière mise à jour : Février 2026*
