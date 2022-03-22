pipeline {
  agent any
  stages {
    stage('Build And SonarQube analysis') {
      steps {
        withSonarQubeEnv('sq3') {
          script {
            def scannerHome = tool 'ScannerForMSBuild'
            sh "dotnet ${scannerHome}/sonarscanner begin /k:"webapp" /d:sonar.host.url="http://192.168.243.196:31256"  /d:sonar.login="5ea8ccb6408cdfc6bcddd1cdac3dc56d9c3b1aef" " ;
            sh "dotnet build" ;
            sh "dotnet ${scannerHome}/sonarscanner end /d:sonar.login="5ea8ccb6408cdfc6bcddd1cdac3dc56d9c3b1aef"
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
