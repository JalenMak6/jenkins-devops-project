pipeline {
    agent {
    label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "jenkins-devops-project"
        RELEASE  = "1.0.0"
        DOCKER_USER = "604969"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
            cleanWs()
            }
        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', url: 'https://github.com/JalenMak6/jenkins-devops-project.git'
            }
            
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
            
        }
        stage("Test Application"){
            steps {
                sh "mvn test"
            }
            
        }
        stage("Sonarqube analysis"){
            steps {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token', installationName: 'sonarqube-scanner') {
                        sh "mvn sonar:sonar"
                }
            }
            
        }
        stage("Quality Gate"){
            steps {
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }
            
        }
        stage("Build & Pull Docker image"){
            steps {
                script{
                docker.withRegistry('',DOCKER_PASS){
                    docker_image = docker.build "${IMAGE_NAME}"
                }
                
                docker.withRegistry('',DOCKER_PASS){
                    docker_image.push("${IMAGE_TAG}")
                    docker_image.push("latest")
                }
            }
            }
            
        }
    }
}
