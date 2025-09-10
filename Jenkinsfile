pipeline {
    agent any
    tools {
        nodejs 'NodeJS 20.19.5'
    }
    parameters {
        choice(name: 'env', choices: ['dev', 'prod'], description: 'Select environment')
    }

    stages {
        stage('Determine Environment') {
            steps {
                script {
                    def targetCluster = ''
                    if (params.env == 'prod') {
                        targetCluster = 'Cluster kube de production'
                    }else if (params.env == 'dev'){
                        targetCluster = 'Cluster kube de dev '
                    }
                    echo "Target Cluster: ${targetCluster}"
                    env.TARGET_CLUSTER = targetCluster
                }
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('install dependencies') {
            steps {
                sh 'npm install'
                sh 'npm install -g @nestjs/cli'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Tests') {
            steps {
                sh 'npm run test -- --ci --reporters=jest-junit'
            }
            post {
                always {
                    junit 'junit.xml'
                }
            }
        }
        
        stage('SonarBookmymovie Analysis') {
            steps {
                script {
                    def mvnHome = tool 'NodeJS 20.19.5'
                    withSonarQubeEnv('SonarBookmymovie') {
                        sh "npx sonar-scanner -Dsonar.projectKey=Bookmymovie -Dsonar.projectName='Bookmymovie' -Dsonar.sources=src -Dsonar.language=ts"
                    }
                }
            }

        }
    }
}
