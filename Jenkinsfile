pipeline {
    agent any
    tools {
        nodejs 'node16'
        //////
    }

    environment {
        sonarScannerHome = tool 'sonar'
    }

    stages {
        stage('Git-checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AslamSunkesula/fullstack-bank.git'
            }
        }
        stage('Clean workspace') {
            steps {
                cleanWs()
                // Ensure proper permissions on workspace
                sh 'sudo chown -R jenkins:jenkins . || true'
                sh 'sudo chmod -R 755 . || true'
            }
        }

        // stage('Dependency-check') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }

        // stage('TRIVY FS SCAN') {
        //     steps {
        //         sh "trivy fs --format table -o trivy-fs-report.html ."
        //     }
        // }

        // stage('Sonar-scanner') {
        //     steps {
        //         withSonarQubeEnv('sonar-server') {
        //             sh "$sonarScannerHome/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank"
        //         }
        //     }
        // }

        stage('Install dependencies') {
            steps {
                script {
                    // Use --unsafe-perm flag for root-related permission issues
                    sh 'npm install --unsafe-perm --prefix }'
                }
            }
        }

        stage('Backend build') {
            steps {
                dir('/var/lib/jenkins/workspace/bank-app/app/backend') {
                sh 'npm install --unsafe-perm'                }
            }
        }

        stage('Frontend build') {
            steps {
                dir('/var/lib/jenkins/workspace/bank-app/app/frontend') {
                sh 'npm install --unsafe-perm'
                }
            }
        }
        stage('Deployment') {
            steps {
                // Ensure docker-compose has proper permissions
                sh 'docker compose -f docker-compose.yml up -d --build'
            }
        }
    }

}