 pipeline {
    agent any
    stages{
        stage('checkout'){
            steps{
                echo "*********** cloning the code **********"
                sh 'rm -rf Calci || true'
                sh 'git clone  https://github.com/Siva825/Calci.git'     
            }
        }
        stage('Artifact build'){
            steps{
                echo "********** building is done ************"
                dir('Calci/calculator'){
                    sh'mvn clean package -DskipTests -Dcyclonedx.skip=true -Dcheckstyle.skip=true'
                }
            }
        }
    }
}
