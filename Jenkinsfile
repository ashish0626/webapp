pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          script {
            def scannerHome = tool 'ScannerForMSBuild'
            sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"my-project-key\" " ;
            sh "dotnet build" ;
            sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"
	  }
        }
      }
    }

    stage("Quality Gate") {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
          // true = set pipeline to UNSTABLE, false = don't
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}
