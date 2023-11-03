pipeline {
    agent any
    
    environment {
        VeracodeID = '9c42b4bd8abf978e9dc1667451778c58'
        VeracodeKey    = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
        VeracodeProfile = 'Arun-jenkins-test'
        CaminhoPacote = 'target/verademo.war'
    }

    stages {
        stage('Git Clone') {
            steps {
                git "https://github.com/IGDEXE/Verademo"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Veracode Upload And Scan') {
            steps {
                sh 'curl -o veracode-wrapper.jar https://repo1.maven.org/maven2/com/veracode/vosp/api/wrappers/vosp-api-wrappers-java/23.4.11.2/vosp-api-wrappers-java-23.4.11.2.jar'
                sh 'java -jar veracode-wrapper.jar -vid ${VeracodeID} -vkey ${VeracodeKey} -action uploadandscan -appname ${VeracodeProfile} -createprofile true  -version $(date +%H%M%s%d%m%y) -filepath ${CaminhoPacote}'
            }
        }
    }
}