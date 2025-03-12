# Jenkins DevSecOps Pipeline

This project is the extension of project [Wanderlust](https://github.com/krishnaacharyaa/wanderlust) . This repo contains the Jenkins file to automate the DevSecOps pipeline for the Wanderlust project. The pipeline includes code quality analysis, dependency vulnerability scanning, security scanning, and containerized deployment.
Ensure the following tools are installed and configured before running the pipeline:

- **Jenkins** (with necessary plugins installed)
- **SonarQube** (for code quality analysis)
- **OWASP Dependency Check** (for dependency vulnerability scanning)
- **Trivy** (for security scanning)
- **Docker & Docker Compose** (for containerized deployment)
- **Git** (for cloning the repository)

## Pipeline Stages and Tools Used

### 1. Code Clone from GitHub
**Tool:** Git
- Clones the repository from GitHub using the specified branch (`devops`).

### 2. SonarQube Quality Analysis
**Tool:** SonarQube
- Scans the code using SonarQube for static code analysis.
- Uses the SonarQube scanner to analyze the project and send results to the SonarQube server.

### 3. OWASP Dependency Check
**Tool:** OWASP Dependency Check
- Scans the project dependencies for known vulnerabilities.
- Publishes a report (`dependency-check-report.xml`).

### 4. Sonar Quality Gate Scan
**Tool:** SonarQube
- Waits for SonarQube's Quality Gate status to ensure the code meets defined quality standards.
- The pipeline will proceed only if the Quality Gate passes.

### 5. Trivy File System Scan
**Tool:** Trivy
- Scans the file system for vulnerabilities.
- Generates a report (`trivy-fs-report.html`).

### 6. Deploy using Docker Compose
**Tool:** Docker & Docker Compose
- Deploys the application using Docker Compose.
- Runs `docker-compose up -d` to start the services in detached mode.

## Environment Variables

- **SONAR_HOME**: Path to the SonarQube installation.
- **NVD_API_KEY**: API key for accessing the National Vulnerability Database (used in OWASP Dependency Check).

## Running the Pipeline

1. Ensure Jenkins is running with all necessary plugins installed.
2. Configure SonarQube, OWASP Dependency Check, and Trivy in Jenkins.
3. Run the pipeline manually or set up a trigger for automatic execution.
4. Check the reports generated by SonarQube, OWASP Dependency Check, and Trivy for any issues.
5. If all quality checks pass, the application is deployed via Docker Compose.

## Expected Output

- Code analysis and quality gate results in SonarQube.
- Security reports from OWASP Dependency Check and Trivy.
- Running application deployed via Docker Compose.

## Troubleshooting

- **SonarQube Analysis Fails:** Check if the SonarQube server is running and the project key is correct.
- **OWASP Dependency Check Fails:** Ensure an API key is provided for accessing vulnerability databases.
- **Trivy Scan Errors:** Verify Trivy installation and update vulnerability databases if needed.
- **Deployment Issues:** Check Docker logs (`docker-compose logs -f`) for troubleshooting.

