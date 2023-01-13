pipeline {
  agent any
  stages {
      stage ('Test') {
          steps {
            bat 'gradle test';
            bat 'gradle check';
            cucumber buildStatus: 'UNSTABLE',
                                  fileIncludePattern: '**/*.json',
                                  trendsLimit: 10,
                                  classifications: [
                                      [
                                          'key': 'Browser',
                                          'value': 'Firefox'
                                      ]
                                  ]
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
                echo "Build stage complete"
              }
              failure {
                echo "Deployment failed"
                mail to: "ryan.belbachir01@gmail.com",
                subject: "Deployment failed",
                body: "Deployment failed"
              }
              success {
                echo "Deployment succeeded";
                mail to: "ryan.belbachir01@gmail.com",
                subject: "Deployment succeeded",
                body: "Deployment succeeded"
                notifyEvents message: 'Hello folks : <b>Deployment succeeded</b> ! ', token: 'U0EjI1wk1BDWAgmpAElBmcxuXNBVyHmo'
              }
    }

}