pipeline {
  agent any

  environment {
    VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
    VERACODE_API_SECRET = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
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
        script {
          sh """
            curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip
            unzip -o pipeline-scan-LATEST.zip pipeline-scan.jar
            java -jar pipeline-scan.jar \\
            --veracode applicationName: 'Arun-jenkins-test', criticality: 'Medium', deleteIncompleteScanLevel: '0', fileNamePattern: '', replacementPattern: '', sandboxName: 'build', scanExcludesPattern: '', scanIncludesPattern: '', scanName: 'Arun-jenkins-test', teams: '', uploadIncludesPattern: '**/**.jar', vid: '9c42b4bd8abf978e9dc1667451778c58', vkey: '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
          """
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