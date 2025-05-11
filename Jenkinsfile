pipeline {
    agent any
    tools {
        nodejs 'node16'
    }

    environment {
        sonarScannerHome = tool 'sonar'
    }

    stages {
        stage('Git-checkout') {
            steps {
                dir('bank-app') {
                    git branch: 'main', url: 'https://github.com/AslamSunkesula/fullstack-bank.git'
                }
            }
        }

        stage('Clean workspace') {
            steps {
                cleanWs()
                sh 'sudo chown -R jenkins:jenkins . || true'
                sh 'sudo chmod -R 755 . || true'
            }
        }

        stage('Debug workspace') {
            steps {
                sh 'ls -la'
                sh 'ls -la bank-app'
            }
        }

        stage('Install dependencies') {
            steps {
                dir('bank-app') {
                    sh 'npm install --unsafe-perm'
                }
            }
        }

        stage('Backend build') {
            steps {
                dir('bank-app/app/backend') {
                    sh 'npm install --unsafe-perm'
                    // Uncomment the following line if a build step is required
                    // sh 'npm run build'
                }
            }
        }

        stage('Frontend build') {
            steps {
                dir('bank-app/app/frontend') {
                    sh 'npm install --unsafe-perm'
                    // Uncomment the following line if a build step is required
                    // sh 'npm run build'
                }
            }
        }

        stage('Deployment') {
            steps {
                dir('bank-app') {
                    sh 'docker compose -f docker-compose.yml up -d --build'
                }
            }
        }
    }
}