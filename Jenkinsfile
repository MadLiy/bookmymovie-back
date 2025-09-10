pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        success {
            echo "✅ Backend build & tests succeeded"
        }
        failure {
            echo "❌ Backend pipeline failed"
        }
    }
}
