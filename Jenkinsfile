pipeline {
    agent any

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

        stage('Trivy FS Scan') {
            steps {
                sh 'trivy fs .'
            }
        }
    }
}