pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '432547390341.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
    registryCredential = 'jenkins-ecr'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/adedokunk/Helloword_jan_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
                sleep(4)
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
                sleep(4)
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}