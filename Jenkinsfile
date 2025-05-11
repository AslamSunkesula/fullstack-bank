pipeline {
    agent any

    tools {
        nodejs 'node16'
    }

    environment {
        SCANNER_HOME = tool 'sonar'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AslamSunkesula/fullstack-bank.git'
            }
        }

        // stage('OWASP FS SCAN') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }

        // stage('TRIVY FS SCAN') {
        //     steps {
        //         sh "trivy fs ."
        //     }
        // }

        // stage('SONARQUBE ANALYSIS') {
        //     steps {
        //         withSonarQubeEnv('sonar') {
        //             sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank"
        //         }
        //     }
        // }

        stage('Install Dependencies (Root)') {
            steps {
                sh "npm install"
            }
        }

        stage('Backend') {
            steps {
                dir('app/backend') {
                    sh "npm install"
                }
            }
        }

        stage('Frontend') {
            when {
                expression { fileExists('app/frontend/package.json') }
            }
            steps {
                dir('app/frontend') {
                    sh "npm install"
                }
            }
        }

        stage('Deploy to Container') {
            steps {
                sh "docker compose up -d"
            }
        }
    }
}
