# GCP Cloud Run CI/CD 

## 📌 Overview

It utilizes **GitHub Actions** to automate the build and deployment of a containerized Node.js application to **Google Cloud Run**, using **Google Artifact Registry (GAR)** for image storage and **Workload Identity Federation (WIF)** for secure, keyless authentication.

## 🏗️ Architecture & Flow
1. **Source Control:** Code is hosted on GitHub.
2. **Authentication:** GitHub Actions authenticates to GCP using OIDC (Workload Identity Federation) — no long-lived JSON service account keys are stored.
3. **Build Pipeline (`build.yml`):** Packages the application into a Docker container and pushes it to Google Artifact Registry.
4. **Deploy Pipeline (`deploy.yml`):** Pulls the latest image from Artifact Registry and deploys it as a serverless container to Google Cloud Run.

## 📁 Repository Structure
```text
.
├── .github/
│   └── workflows/
│       ├── build.yml         # CI workflow: Builds and pushes the Docker image to GAR
│       └── deploy.yml        # CD workflow: Deploys the image to Cloud Run
├── Dockerfile                # Instructions to build the container image
├── package.json              # Node.js dependencies and scripts
└── server.js                 # Dummy application code
```
** Prerequisites & Setup **
1. GCP Infrastructure
To replicate this in another environment, the following GCP resources must be provisioned:
  - **Google Artifact Registry**: A Docker repository to store images.
  - **Workload Identity Federation**: A pool and provider configured to trust GitHub's OIDC tokens.
  - **Service Account**: A dedicated service account for GitHub Actions with the following IAM roles:
      - roles/artifactregistry.writer (To push images)
      - roles/run.developer (To deploy to Cloud Run)
      - roles/run.admin (To allow unauthenticated public access, if required)
      - roles/iam.serviceAccountUser (To act as the runtime service account)

2. GitHub Secrets
    - The following secrets must be configured in the repository settings (Settings > Secrets and variables > Actions):
          - <img width="921" height="318" alt="image" src="https://github.com/user-attachments/assets/17e755db-307f-4a3d-987a-d9631ca13e29" />

  **How to Trigger the Pipelines**
   - the workflows are configured with workflow_dispatch, meaning they are triggered manually via the GitHub UI.
   - Step 1: Build the Image
        - Navigate to the Actions tab in this repository.
        - Select build from the left-hand sidebar.
        - Click the Run workflow dropdown on the right.
        - Select the main branch and click Run workflow.
        - Wait for the workflow to complete successfully.

   - Step 2: Deploy to Cloud Run
        - In the Actions tab, select deploy from the left-hand sidebar.
        - Click the Run workflow dropdown on the right.
        - Select the main branch and click Run workflow.
   Once completed, expand the final step in the logs (Show Output URL) to get the live URL of the deployed application.

Github Actions: 
<img width="1919" height="725" alt="image" src="https://github.com/user-attachments/assets/30c917d4-cd57-44b0-a1d2-125cb6ed8f89" />

Cloud run app
<img width="1912" height="609" alt="image" src="https://github.com/user-attachments/assets/4a65dc28-711f-46c9-a8af-80e75cd9f198" />
Cloud run URL
<img width="1919" height="360" alt="image" src="https://github.com/user-attachments/assets/2f922468-bb92-4b72-83fc-c2d4d97276ea" />
