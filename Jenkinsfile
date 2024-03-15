pipeline {
    agent any

    tools {
        maven "MAVEN3"
    }

    stages {
        stage("Check out") {
            steps {
                git branch: 'main', url: 'https://github.com/drangani007/jenkins-docker-example.git'
            }
        }

        stage("Build Maven Project") {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        stage("Code Coverage (Jacoco)") {
            steps {
                sh "mvn jacoco:report"
            }
        }
        
        stage("Docker Build") {
            steps {
                script {
                    dockerCmd = "\"C:/Program Files/Docker/Docker/resources/bin/docker.exe\""
                    sh "${dockerCmd} build -t dhruvilrangani/my-app-1.0 ."
                }
            }
        }
        
       stage("Docker Login") {
            steps {
                script {
                    dockerCmd = "\"C:/Program Files/Docker/Docker/resources/bin/docker.exe\""
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "${dockerCmd} login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    }
                }
            }
        }

        stage("Docker Push") {
            steps {
                script {
                    dockerCmd = "\"C:/Program Files/Docker/Docker/resources/bin/docker.exe\""
                    sh "${dockerCmd} push dhruvilrangani/my-app-1.0"
                }
            }
        }
    }
}
