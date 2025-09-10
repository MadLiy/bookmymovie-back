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
                    def scannerHome = tool 'SonarQube Scanner 7.2.0'
                    withSonarQubeEnv('SonarBookmymovie') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.token=sqp_5d5a2084e75eca2d0ce78c006a6176207ab2fd4e \
                            -Dsonar.projectKey=Bookmymovie"
                    }

                    timeout(time: 5, unit: 'MINUTES') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline échoué à cause du Quality Gate : ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Security') {
            steps {
                sh '''
                    # Build your project image
                    docker build -t bookmymovie-back:latest .

                    # Scan the image with Trivy
                    trivy image --severity CRITICAL --exit-code 1 bookmymovie-back:latest
                '''
            }
        }
    }
}
