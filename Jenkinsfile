  pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-creds')
    }
    stages{
        stage('checkout'){
            steps{
                echo "*********** cloning the code **********"
                sh 'rm -rf spring-petclinic || true'
                sh 'git clone https://github.com/Siva825/spring-petclinic.git'     
            }
        }
        stage('build'){
            steps{
                echo "********** building is done ************"
                dir('spring-petclinic'){
                    sh'mvn clean package -DskipTests -Dcyclonedx.skip=true'
                }
            }
        }
        stage('Prepare for Docker Build') {
            steps {
                // Archive the built artifacts so they can be transferred to slave
                stash name: 'java-artifact', includes: 'spring-petclinic/target/spring-petclinic-3.5.0-SNAPSHOT.jar'
                stash name: 'Dockerfile', includes: 'spring-petclinic/Dockerfile'
            }
        }
        stage('Build Docker Image') {
            agent { 
                label 'java-slave'
            } 
            steps {
                // Unstash artifacts on the slave node
                unstash 'java-artifact'
                unstash 'Dockerfile'
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
