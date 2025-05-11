pipeline {
    agent any
    tools {
        nodejs 'node16'
        //////
    }

    environment {
        sonarScannerHome = tool 'SonarQube'
    }

    stages {
        stage('Git-checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AslamSunkesula/fullstack-bank.git'
            }
        }

        stage('Dependency-check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('Sonar-scanner') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "$sonarScannerHome/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Backend build') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Frontend build') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/frontend') {
                    sh 'npm install'
                }
            }
        }
        stage('deployment') {
            steps {
               sh 'docker compose -f docker-compose.yml up -d'
            }
    }
}

}