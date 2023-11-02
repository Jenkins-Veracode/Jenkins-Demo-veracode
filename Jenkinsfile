pipeline {
  agent any

  environment {
        VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
        VERACODE_API_SECRET    = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
        CI_TIMEOUT = '20'
    }

  stages {
    stage('Maven Build') {
      steps {
        dir('app') {
          sh 'mvn clean verify'
        }
      }
    }
    stage('Veracode Pipeline Scan') {
      steps {
        sh 'curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip'
        sh 'unzip -o pipeline-scan-LATEST.zip pipeline-scan.jar'
        sh 'java -jar pipeline-scan.jar \
          --veracode_api_id "${VERACODE_API_ID}" \
          --veracode_api_key "${VERACODE_API_SECRET}" \
          --file "build/libs/sample.jar" \
          --fail_on_severity="Very High, High" \
          --fail_on_cwe="80" \
          --timeout "${CI_TIMEOUT}" \
          --project_name "${env.JOB_NAME}" \
          --project_url "${env.GIT_URL}" \
          --project_ref "${env.GIT_COMMIT}"'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'results.json', fingerprint: true
    }
  }
}