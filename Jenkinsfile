 pipeline {
    agent any
    stages{
        stage('checkout'){
            steps{
                echo "*********** cloning the code **********"
                sh 'rm -rf calci || true'
                sh 'git clone  https://github.com/Akash2345ind/calci.git'     
            }
        }
        stage('Artifact build'){
            steps{
                echo "********** building is done ************"
                dir('calci'){
                    sh'mvn clean package -DskipTests -Dcyclonedx.skip=true -Dcheckstyle.skip=true'
                }
            }
        }
    }
}
