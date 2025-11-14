 pipeline {
    agent any
    stages{
        stage('checkout'){
            steps{
                echo "*********** cloning the code **********"
                sh 'rm -rf spring-petclinic || true'
                sh 'git clone https://github.com/Siva825/spring-petclinic.git'     
            }
        }
        stage('Artifact build'){
            steps{
                echo "********** building is done ************"
                dir('spring-petclinic'){
                    sh'mvn clean package -DskipTests -Dcyclonedx.skip=true -Dcheckstyle.skip=true'
                }
            }
        }
    }
}
