# Dockerized ASP.NET Application with Ansible and GitHub Actions

This repository contains a project to containerize an ASP.NET application using Docker and automate its deployment with Ansible and GitHub Actions. The deployment includes a CI/CD pipeline for seamless updates and container management using Watchtower.

## Project Overview

The project demonstrates the following capabilities:

- **Dockerization**: ASP.NET application is packaged into a Docker container.
- **Deployment Automation**: Deployment is automated using Ansible playbooks.
- **CI/CD Integration**: GitHub Actions handle build, test, and deployment pipelines.
- **Container Management**: Watchtower ensures containers are automatically updated.

## File Structure

```
.
├── .github/workflows
│   ├── build-docker-image.yml    # CI workflow to build Docker images
│   ├── deploy.yml                # CD workflow to deploy the application
├── TodoApi                       # ASP.NET Core application
│   ├── .vscode                   # Visual Studio Code configuration
│   ├── Controllers               # API Controllers
│   ├── Properties                # Project properties
│   ├── bin/Debug/net8.0          # Build output (ignored in repo)
│   ├── obj                       # Build object files (ignored in repo)
│   ├── .dockerignore             # Docker ignore file
│   ├── .gitignore                # Git ignore file
│   ├── Dockerfile                # Docker build instructions
│   ├── Program.cs                # Main entry point of the application
│   ├── TodoApi.csproj            # Project file
│   ├── TodoApi.http              # HTTP request examples
│   ├── TodoApi.sln               # Solution file
│   ├── WeatherForecast.cs        # Example model class
│   ├── appsettings.Development.json # Development configuration
│   ├── appsettings.json          # Default configuration
│   ├── docker-compose.debug.yml  # Debug mode compose file
│   ├── docker-compose.yml        # Docker Compose file
├── ansible                       # Ansible deployment configuration
│   ├── roles                     # Ansible roles
│   │   ├── app                   # Application deployment role
│   │   ├── docker/tasks          # Role to install Docker
│   │   ├── stop-container/tasks  # Role to stop existing containers
│   │   ├── watchtower            # Role to deploy Watchtower
│   ├── ansible.cfg               # Ansible configuration
│   ├── deploy_todoapi.yml        # Main playbook for deployment
│   ├── inventory.yml             # Inventory of target hosts
└── README.md                     # Project documentation
```

## Key Features

### Dockerization
- The ASP.NET application is built into a container using a Dockerfile.
- Multi-stage builds can be used for optimized images.

### Ansible Playbooks
- **Install Docker**: Ensures Docker is installed on target hosts.
- **Remove Existing Containers**: Stops and removes any existing containers before deployment.
- **Deploy Application**: Builds and runs the containerized application.
- **Watchtower Integration**: Adds Watchtower for automatic container updates.

### GitHub Actions
- **`build-docker-image.yml`**: Automates building and pushing Docker images.
- **`deploy.yml`**: Deploys the application to target servers using Ansible.

## How to Use

### Prerequisites
- Docker and Docker Compose installed locally.
- Ansible installed on the deployment system.
- GitHub repository with CI/CD workflows configured.

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/mikky-ms/aspdotnet.git
   ```
2. Navigate to the project directory:
   ```bash
   cd aspdotnet
   ```
3. Build the Docker image:
   ```bash
   docker build -t todoapi:latest ./TodoApi
   ```
4. Run the application locally:
   ```bash
   docker-compose up
   ```

### Deployment
1. Configure the Ansible `inventory.yml` file with your target hosts.
2. Run the Ansible playbook:
   ```bash
   ansible-playbook -i inventory.yml ansible/deploy_todoapi.yml
   ```
3. Monitor logs and ensure the deployment is successful.

### CI/CD Pipeline
- Push changes to the GitHub repository to trigger the CI/CD workflows.
- `build-docker-image.yml` builds and pushes the Docker image.
- `deploy.yml` handles automated deployment.
