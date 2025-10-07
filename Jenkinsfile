pipeline {
    agent {
        dockerfile {
            dir 'agent'
        }
    }

    environment {
        scannerHome = tool 'SonarScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Analyse SCA (Dépendances)') {
            steps {
                sh 'npm install'
                sh 'npm audit --audit-level=high'
            }
        }

        stage('Analyse SAST (SonarQube)') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage('Vérification Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build & Scan Image Docker') {
            steps {
                script {
                    def imageTag = "mon-app-pipeline:${env.BUILD_ID}"
                    def dockerImage = docker.build(imageTag)
                    sh "trivy image --exit-code 1 --severity CRITICAL ${imageTag}"
                }
            }
        }
    }
}
