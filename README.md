# Vprofile App – CI/CD Pipeline

This repository contains the source code along with **Continuous Integration (CI)** and **Continuous Deployment (CD)** configurations for the **Vprofile App**.  
The pipeline automates code analysis, artifact creation, publication, and deployment—leveraging AWS services for a secure, repeatable, and scalable delivery process.

---

## 🚀 Architecture Overview

![Pipeline Architecture Diagram](AWS-Ci.png) <!-- Replace with actual image path if different -->

According to the diagram above, the workflow proceeds as follows:

1. **Push Code**  
   Developer pushes changes to **Bitbucket**.

2. **Pipeline Trigger**  
   **AWS CodePipeline** triggers automatically on every commit and orchestrates the workflow.

3. **Code Analysis**  
   - **AWS CodeBuild** downloads dependencies from **AWS CodeArtifact**.  
   - Runs **unit tests**, **Checkstyle**, and **SonarCloud** analysis.  
   - Pipeline halts if the **SonarCloud quality gate** fails.

4. **Build Artifact**  
   - **AWS CodeBuild** compiles and packages the application.  
   - The generated `.war` artifact is pushed to an **AWS S3 bucket**.

5. **Software Testing (Development Environment)**  
   - The artifact is deployed to **AWS Elastic Beanstalk (Dev)**.  
   - The app connects to a managed **Amazon RDS MySQL** instance for its database layer.  
   - Automated or manual tests can be executed to validate the build.

6. **Deploy to Production**  
   - After successful validation, the same artifact is promoted and deployed to **AWS Elastic Beanstalk (Prod)**, which also uses **Amazon RDS MySQL** for production data.

> ✅ **All Maven dependencies are securely fetched from AWS CodeArtifact to ensure reproducible builds.**  
> ✅ **Application database is hosted on Amazon RDS (MySQL) for reliability, backups, and scalability.**

---

## ✅ Prerequisites

| Requirement | Notes |
|-------------|-------|
| **AWS Account** | IAM permissions for CodePipeline, CodeBuild, S3, CodeArtifact, RDS |
| **Amazon RDS (MySQL)** | A provisioned MySQL instance accessible from Elastic Beanstalk environments |
| **SonarCloud Account** | Needed for static analysis and quality gate integration |
| **Java 17** | Required for local development and build |
| **Maven 3+** | Understanding of `settings.xml`, profiles, etc. |

---

## 🛠️ Technologies Used

| Purpose                      | Service / Tool          |
|------------------------------|-------------------------|
| CI/CD Orchestration          | **AWS CodePipeline**    |
| Build & Analysis             | **AWS CodeBuild**       |
| Artifact Storage             | **AWS S3**              |
| Dependency Management        | **AWS CodeArtifact**    |
| Static Code Analysis         | **SonarCloud**, **Checkstyle** |
| Build Automation             | **Apache Maven**        |
| Application Runtime          | **Java (Corretto 17)**  |
| JSON Processing in Builds    | **jq**                  |
| Deployment Target            | **AWS Elastic Beanstalk** |
| Managed Database             | **Amazon RDS (MySQL)**  |

---

## 📌 Database

The application database is hosted on **Amazon RDS for MySQL**.  
You can import the initial schema and data using the included dump file if needed:

- `src/main/resources/db_backup.sql`

To restore the dump locally or into RDS (via an accessible bastion or connection):

```bash
mysql -h <rds-endpoint> -u <username> -p <database_name> < db_backup.sql
