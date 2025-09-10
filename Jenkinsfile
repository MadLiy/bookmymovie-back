pipeline {
    agent { label 'build-heavy' }
    stages {
        stage('Build Back') {
            steps {
                echo "Build back..."
            }
        stage('Maven Clean')
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
