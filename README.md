# GeneconnectBE CI/CD Pipeline

## 1. Overview
 
This repository contains the Azure DevOps pipeline configuration for the **GeneconnectBE** application. The primary goal is to establish a robust, manually-triggered Continuous Integration and Continuous Deployment (CI/CD) pipeline.

The pipeline automates the process of building the application's Docker image, versioning it systematically, and publishing it to a secure Azure Container Registry (ACR). This process ensures consistent and reliable deployments.

## 2. Pipeline Workflow

The pipeline executes the following sequence of steps upon manual invocation:

1.  **Clone Source Code**: Clones the application source code from the private GitHub repository (`AryanShorthillsAI/GeneconnectBE`).
2.  **Fetch Build Secrets**: Securely connects to the production EC2 instance via SSH to download the `Dockerfile` and the `.env` configuration file. These files are essential for the build but are not stored in the source code repository for security reasons.
3.  **Versioning**:
    *   Connects to an Azure Blob Storage container to read a `version.txt` file.
    *   Increments the integer value within the file.
    *   Calculates a semantic version tag (e.g., `v1.0.1`) based on the new integer.
    *   Uploads the updated `version.txt` file back to Blob Storage.
4.  **Build Docker Image**: Builds a new Docker image using the fetched `Dockerfile` and the application source code.
5.  **Tag and Push to ACR**:
    *   Logs into the Azure Container Registry (ACR).
    *   Tags the newly built Docker image with both the calculated version tag (e.g., `v1.0.1`) and a `latest` tag.
    *   Pushes both tags to the ACR, making the new build available for deployment.

## 3. Technology Stack

*   **CI/CD Platform**: Azure DevOps Pipelines
*   **Container Registry**: Azure Container Registry (ACR)
*   **State & Versioning**: Azure Blob Storage
*   **Containerization**: Docker
*   **Source Control**: GitHub

## 4. Setup and Configuration

To successfully run this pipeline, the following resources and configurations must be in place within your Azure and Azure DevOps environment.

### Azure Prerequisites

1.  **Azure Container Registry (ACR)**: A registry to store the Docker images.
2.  **Azure Storage Account**: A storage account with a blob container (e.g., `versioning`) to hold the `version.txt` file.
3.  **Service Principal**: An Azure Active Directory service principal with `Contributor` rights scoped to the resource group containing the ACR and Storage Account. This allows the pipeline to authenticate securely.

### Azure DevOps Prerequisites

1.  **Project**: An Azure DevOps project.
2.  **Service Connections**:
    *   **Azure Resource Manager**: A connection to your Azure subscription using the created Service Principal.
    *   **GitHub**: A connection to your GitHub account to access the `GeneconnectBE` repository.
3.  **Secure Files**: The private SSH key required to access the EC2 instance must be uploaded to **Pipelines -> Library -> Secure Files**.

## 5. How to Run the Pipeline

This pipeline is designed for manual execution:

1.  Navigate to the **Pipelines** section in your Azure DevOps project.
2.  Select the `GeneconnectBE` pipeline.
3.  Click the **Run pipeline** button.
4.  Confirm the branch and click **Run**.
