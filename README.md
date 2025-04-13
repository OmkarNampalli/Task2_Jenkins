# Jenkins CI/CD Pipeline Project

## What's Included
- Jenkinsfile for CI/CD using Docker
- Dockerfile for containerizing the app
- Node.js sample app

## Pipeline Stages
1. **Checkout**: Pulls the latest code from GitHub
2. **Build**: Builds a Docker image
3. **Test**: Runs unit tests inside the container
4. **Push**: Pushes Docker image to Docker Hub
5. **Deploy**: Runs container with the new image

## How to Use
- Configure Jenkins with Docker and credentials
- Create a pipeline job linked to this repo
- Push changes to GitHub to trigger pipeline
# Jenkins_task
