pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Code Clone from GitHub") {
            steps {
                git url: "https://github.com/krishnaacharyaa/wanderlust", branch: "devops"
            }
        }
        stage("SonarQube Quality Analysis") {
            steps {
                withSonarQubeEnv("Sonar") {
                    sh "${SONAR_HOME}/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
       stage("OWASP Dependency Check") {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --nvdApiKey ${NVD_API_KEY}', odcInstallation: 'owasp'
                dependencyCheckPublisher pattern: "**/dependency-check-report.xml"
            }
        }
        stage("Sonar Quality Gate Scan") {
            steps {
                timeout(time: 2, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System Scan") {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Deploy using Docker compose"){
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
