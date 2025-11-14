pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-creds')
    }
    stages {
        stage('Build Java Artifact') {
            steps {
                sh 'rm -rf spring-petclinic || true'
                sh 'git clone https://github.com/Siva825/spring-petclinic.git'
                dir 'spring-petclinic' {
                sh 'mvn clean package -DskipTests'
                }
            }
        }
        stage('Build and Push Docker Image') {
            agent { 
                label 'java-slave'
            } 
            steps {
                 sh 'docker build -t siva2626/springpetclinic:v1 .'
            }
        }
        stage('Push to Docker Hub') {
            agent { 
                label 'java-slave'
           } 
            steps {
               sh """
                docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}
                docker push siva2626/springpetclinic:v1
                """
            }
        }   
    }
}
