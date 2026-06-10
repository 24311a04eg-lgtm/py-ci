Production-Ready CI/CD Pipeline for Python Applications

A complete CI/CD workflow built with GitHub Actions, Docker, and AWS ECR.

This project automates the entire software delivery process — from linting and testing to container validation and cloud image deployment.

The goal of this project was to simulate a real-world DevOps pipeline used in production environments while keeping the workflow simple, practical, and easy to understand.

What This Pipeline Does

Every time code is pushed to the main branch or a pull request is opened against main:

* Runs lint checks using Flake8
* Executes automated tests with Pytest
* Builds a Docker image
* Scans the image for vulnerabilities using Trivy
* Runs the container automatically
* Verifies application health with an API endpoint
* Saves and transfers the Docker image between workflow stages
* Pushes the validated image to AWS ECR

Everything is handled through a single GitHub Actions workflow.

⸻

Tech Stack

* Python 3.11
* GitHub Actions
* Docker
* AWS ECR
* Flake8
* Pytest
* Trivy

⸻

CI/CD Workflow

Code Push / Pull Request
            ↓
      Lint Validation
            ↓
      Automated Tests
            ↓
      Docker Build
            ↓
     Security Scanning
            ↓
   Container Verification
            ↓
      Health Check Test
            ↓
     Artifact Creation
            ↓
    Push Image to AWS ECR

⸻

Project Structure

py-ci/
│
├── .github/
│   └── workflows/
│       └── main.yml
│
├── app.py
├── requirements.txt
├── Dockerfile
├── tests/
└── README.md

⸻

Workflow Breakdown

1. Lint Stage

The pipeline starts by checking code quality using Flake8.

flake8 .

This helps catch:

* Syntax issues
* Formatting problems
* Bad coding practices

before moving further into the pipeline.

⸻

2. Test Stage

After linting passes, automated tests are executed using Pytest.

pytest -v

This ensures the application behaves correctly before creating deployment artifacts.

The workflow also uses GitHub Actions cache to speed up dependency installation between runs.

⸻

3. Docker Build & Validation

The application is containerized using Docker.

docker build -t repo:latest .

Once the image is built, the pipeline:

* Scans the image using Trivy
* Starts the container
* Displays running containers
* Inspects the container
* Shows container logs
* Verifies the /health endpoint

Health Check:

curl -v http://localhost:5000/health

This step validates that the application is actually running correctly inside the container before deployment.

⸻

Security Scanning

The workflow uses Trivy to scan Docker images for known vulnerabilities.

This adds an important DevSecOps practice directly into the CI/CD pipeline.

⸻

Docker Image Artifact Handling

After validation, the Docker image is saved as a tar archive and uploaded as a GitHub Actions artifact.

docker save repo:latest -o repo.tar

The deploy stage downloads the artifact and loads the image back into Docker before deployment.

docker load -i repo.tar

This demonstrates artifact sharing between isolated GitHub Actions jobs.

⸻

AWS ECR Deployment

After successful validation, the Docker image is pushed to Amazon Elastic Container Registry (ECR).

Deployment flow:

GitHub Actions
      ↓
Docker Build
      ↓
Security Scan
      ↓
Artifact Upload
      ↓
Artifact Download
      ↓
AWS ECR Push

AWS credentials are securely stored using GitHub Secrets.

Required secrets:

* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY

AWS Region:

ap-southeast-2

⸻

Why This Project Matters

This project demonstrates how modern deployment pipelines reduce manual work and improve reliability.

Instead of:

* manually testing applications,
* manually building images,
* manually validating containers,
* and manually deploying,

the entire workflow is automated using GitHub Actions.

This creates:

* faster deployments,
* consistent environments,
* repeatable builds,
* and fewer human errors.

⸻

Running Locally

Install Dependencies

pip install -r requirements.txt

Run the Application

python app.py

Build Docker Image

docker build -t repo:latest .

Run Docker Container

docker run -d -p 5000:5000 repo:latest

⸻

Future Improvements

Planned improvements for this project:

* ECS deployment automation
* Kubernetes integration
* Multi-stage Docker builds
* Terraform infrastructure setup
* Monitoring & logging
* Slack notifications
* Blue-Green deployments

⸻

Key DevOps Concepts Used

* Continuous Integration
* Continuous Deployment
* Docker Containerization
* Security Scanning
* Artifact Management
* Health Monitoring
* Cloud Registry Deployment
* GitHub Actions Caching

⸻

Author

Built as a practical DevOps learning project focused on real CI/CD workflows using Docker, GitHub Actions, and AWS.