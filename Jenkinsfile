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
  }

  stages {
    stage('Maven Build') {
      steps {
        dir('app') {
          sh 'mvn clean package'
        }
      }
    }
    stage('Veracode Pipeline Scan') {
      steps {
        script {
            sh 'curl -sSO https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip'
            sh 'unzip -o pipeline-scan-LATEST.zip'
            sh 'java -jar pipeline-scan.jar -vid ${VERACODE_API_ID} -vkey ${VERACODE_API_SECRET} -f "app/target/verademo.war" --issue_details true'
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'results.json', fingerprint: true
    }
  }
}