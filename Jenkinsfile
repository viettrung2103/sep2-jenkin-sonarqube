pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'JDK 21'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins ( in Manage  Jenkin, System)
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

    }
}
