pipeline {
    agent any
//
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
    }
}