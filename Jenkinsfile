// This pipeline will build the jar file
// Download veracode pipeline scan and scan the war file
// Results.json will be created

pipeline {
  agent any

  environment {
    VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
    VERACODE_API_SECRET = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
    CI_TIMEOUT = '20'
    JOB_NAME = 'Jenkins_pipeline'
    filepath = 'app/target/verademo.war'
    VeracodeProfile = 'VeraDemo'
  }

  stages {
    stage('Maven Build') {
      steps {
        dir('app') {
          sh 'mvn clean package'
        }
      }
    }
    stage('Veracode Upload And Scan') {
        steps {
            sh 'curl -o veracode-wrapper.jar https://repo1.maven.org/maven2/com/veracode/vosp/api/wrappers/vosp-api-wrappers-java/23.4.11.2/vosp-api-wrappers-java-23.4.11.2.jar'
            sh 'java -jar veracode-wrapper.jar -vid ${VERACODE_API_ID} -vkey ${VERACODE_API_SECRET} -action uploadandscan -appname ${VeracodeProfile} -createprofile false  -version $(date +%H%M%s%d%m%y) -filepath ${filepath}'
            }
        }


    stage('Container Scan') {
      steps {
        sh '''
          export VERACODE_API_KEY_ID=${VERACODE_API_ID}
          export VERACODE_API_KEY_SECRET=${VERACODE_API_SECRET}
          curl -fsS https://tools.veracode.com/veracode-cli/install | sh
          ./veracode scan --type directory --source . --format table 2 >&1 | tee container-scan.json
        '''
      }
    }
  }
}