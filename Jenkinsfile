pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'JDK 21'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins ( in Manage  Jenkin, System)
        SONAR_TOKEN = 'sqa_5d0321af177ddb94c545897ebb3cec43af18dd1c' // Store the token securely
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub_Trung'
        DOCKERHUB_REPO = 'viettrung21/jenkin-sonarqube'
        DOCKER_IMAGE_TAG = 'latest_v1'
        // Set PATH explicitly for Jenkins
        PATH = "/usr/local/bin:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/viettrung2103/sep2-jenkin-sonarqube.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                  sh """
                      /opt/homebrew/bin/sonar-scanner \
                      -Dsonar.projectKey=devops-demo \
                      -Dsonar.sources=src \
                      -Dsonar.projectName=DevOps-Demo \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.login=${env.SONAR_TOKEN} \
                      -Dsonar.java.binaries=target/classes
                  """
                }
            }
        }
        stage('Docker Login') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS_ID,
                                                             usernameVariable: 'DOCKERHUB_USER',
                                                             passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                                sh '''
                                    echo "$DOCKERHUB_PASSWORD" | docker login -u "$DOCKERHUB_USER" --password-stdin
                                '''
                            }
                        }
                    }
                }
                stage('Build Docker Image') {
                     steps {
                         script {
                             sh """
                                 docker buildx use mybuilder || docker buildx create --use --name mybuilder
                                 docker buildx build --platform=linux/amd64 -t ${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG} --load .
                             """
                         }
                     }
                 }

                stage('Push Docker Image to Docker Hub') {
                    steps {
                        script {
                            sh "docker push ${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}"
                        }
                    }
                }

    }
}
