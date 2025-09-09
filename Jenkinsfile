pipeline {
    agent { label 'build-heavy' }
    stages {
        stage('Build Back') {
            steps {
                echo "Build back..."
            }
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
