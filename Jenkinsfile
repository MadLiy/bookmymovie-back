pipeline {
    agent { label 'build-heavy' }
    stages {
        stage('Build Back') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
