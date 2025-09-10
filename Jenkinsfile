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
                sh 'npm run test -- --ci'
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
                        sh "sonar \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.token=sqp_5d5a2084e75eca2d0ce78c006a6176207ab2fd4e \
                            -Dsonar.projectKey=Bookmymovie"
                    }
                }
            }

        }
    }
}
