pipeline {
  agent any
  stages {
      stage ('Test') {
          steps {
            bat 'gradle test';
            bat 'gradle check';
            junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
            cucumber 'target/report.json'
          }
      }
      stage ('Code Analysis') {
          steps {
            withSonarQubeEnv('SonarQube') {
                bat "gradle sonarqube";
            }
          }
      }
      stage("Quality gate") {
          steps {
            waitForQualityGate abortPipeline: true;
          }
      }
      stage ("Build") {
        steps {
            bat "gradle jar";
            bat "gradle javadoc";
            archiveArtifacts 'build/libs/*.jar';
        }
      }
      stage ("Deploy") {
        steps {
            bat "gradle publish";
        }
      }
}
post {
      always {
        echo "End of Pipeline process"
        mail(subject: 'Process Pipeline : Result incoming ...', body: 'Process Pipeline : Result incoming ...', from: 'jr_belbachir@esi.dz', to: 'ryan.belbachir01@gmail.com')
      }
      failure {
        echo "Deployment failed"
        mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'jr_belbachir@esi.dz', to: 'ryan.belbachir01@gmail.com')
      }
      success {
        echo "Deployment succeeded"
        mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'jr_belbachir@esi.dz', to: 'ryan.belbachir01@gmail.com')
        notifyEvents message: 'Hello folks : <b>Deployment succeeded</b> ! ', token: 'U0EjI1wk1BDWAgmpAElBmcxuXNBVyHmo'
      }
    }
}