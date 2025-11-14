  pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-creds')
    }
    stages {
        stage('checkout') {
            steps {
                echo "*********** cloning the code **********"
                sh 'rm -rf Calci || true'
                sh 'git clone https://github.com/Siva825/Calci.git'     
            }
        }
        
        stage('Artifact build') {
            steps {
                echo "********** building is done ************"
                dir('Calci/calculator') {
                    sh 'mvn clean package -DskipTests -Dcyclonedx.skip=true -Dcheckstyle.skip=true'
                }
            }
        }
        
        stage('Prepare for Docker Build') {
            steps {
                dir('Calci/calculator/target') {
                    stash name: 'java-artifact', includes: 'calculator-0.0.1-SNAPSHOT.jar'
                }
                dir('Calci/calculator') {
                    stash name: 'Dockerfile', includes: 'Dockerfile'
                }
            }
        }
        
        stage('Build Docker Image') {
            agent { 
                label 'java-slave'
            } 
            steps {
                // Unstash artifacts on the slave node
               dir('/home/sivapk188') {
                    unstash 'java-artifact'
                    unstash 'Dockerfile'
                    
                    // Build Docker image
                    sh '''
                    sudo docker build -t siva2626/calculator:v1 .
                    '''
                }
            }
        }
        
        stage('Push to Docker Hub') {
            agent { 
                label 'java-slave'
            } 
            steps {
                sh """
                sudo docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}
                sudo docker push siva2626/calculator:v1
                """
            }
        }
    }
}
