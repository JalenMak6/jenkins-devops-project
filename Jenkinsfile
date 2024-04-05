pipeline {
    agent {
    label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
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
    }
}
