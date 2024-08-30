pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'zap-cli start && zap-cli scan'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh './deploy-staging.sh'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                sh 'selenium-tests.sh'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh './deploy-production.sh'
            }
        }
    }

    post {
        success {
            mail to: 'disuru.office@gmail.com',
                 subject: "Pipeline Completed Successfully",
                 body: "All stages completed successfully. Logs are attached.",
                 attachLog: true
        }
        failure {
            mail to: 'disuru.office@gmail.com',
                 subject: "Pipeline Failed",
                 body: "The pipeline failed. Please check the logs.",
                 attachLog: true
        }
    }
}
