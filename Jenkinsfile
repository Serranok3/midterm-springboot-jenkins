pipeline {
    agent any
    environment {
        NEXUS_CREDS = credentials('nexus-creds')
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw -B clean package -DskipTests'
            }
        }
        stage('Test') {
            steps { sh './mvnw -B test' }
        }
        stage('Archive') {
            steps { archiveArtifacts artifacts: 'target/*.jar', fingerprint: true }
        }
        stage('Push to Nexus') {
            steps { sh './mvnw -B deploy -DskipTests -s settings.xml' }
        }
    }
}
