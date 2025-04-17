pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'JDK 17'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins ( in Manage  Jenkin, System)
        SONAR_TOKEN = 'sqa_c9f9a81b335821eaaf466b3776db80710fc14226' // Store the token securely
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
                        sonar-scanner ^
                        -Dsonar.projectKey=devops-demo ^
                        -Dsonar.sources=src ^
                        -Dsonar.projectName=DevOps-Demo ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=${env.SONAR_TOKEN} ^
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

    }
}
