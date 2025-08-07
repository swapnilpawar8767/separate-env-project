# 🚀 separate-env-project

This project demonstrates a CI/CD pipeline using **GitHub Actions**, **GitHub Container Registry (GHCR)**, and **EC2** instances for separate **Development** and **Production** environments.

---

## 📦 Overview

The application is a static HTML + CSS website, deployed in Docker containers to different EC2 instances based on the branch:

- **Development** deployment: triggers on `dev` branch push.
- **Production** deployment: triggers on `main` branch merge.

The entire workflow is automated via GitHub Actions.

---

## 🛠️ Tech Stack

- **Frontend**: HTML, CSS
- **Containerization**: Docker
- **CI/CD**: GitHub Actions
- **Image Registry**: GitHub Container Registry (GHCR)
- **Deployment Target**: AWS EC2 (Dev & Prod)

---

## 📂 Project Structure

separate-env-project/
>
>├── .github/workflows/
>        ├── dev-deploy.yml
>
>        └── prod-deploy.yml
├── Dockerfile
├── index.html
├── style.css
└── README.md

---

## ⚙️ GitHub Actions Workflow

### ✅ Dev Environment

- Trigger: Push to `dev` branch
- Jobs:
  - Checkout code
  - Build Docker image and tag as `:dev`
  - Push image to GHCR
  - SSH into Dev EC2 and run the container

### ✅ Prod Environment

- Trigger: Push to `main` (usually via PR merge)
- Jobs:
  - Same steps, but uses tag `:prod`
  - Deploys to a separate Production EC2 instance

### 🔒 Secrets Used

Set these in your GitHub repo under **Settings → Secrets and variables → Actions**:

| Secret Name         | Description                          |
|---------------------|--------------------------------------|
| `GHCR_USERNAME`     | Your GitHub username                 |
| `GHCR_PAT`          | GitHub PAT with `write:packages`     |
| `EC2_SSH_USER`      | SSH username for EC2 (e.g. `ubuntu`) |
| `EC2_SSH_KEY`       | SSH private key for the EC2 instance |
| `EC2_DEV_HOST`      | Public IP of Dev EC2 instance        |
| `EC2_PROD_HOST`     | Public IP of Prod EC2 instance       |

---

## 🐳 Docker Usage

**Build Image:**

docker build -t ghcr.io/<username>/separate-env-project:dev .

Run Container:

docker run -d -p 80:80 ghcr.io/<username>/separate-env-project:dev

🌐 Access the App
Dev URL: http://[EC2_DEV_IP]

Prod URL: http://[EC2_PROD_IP]

