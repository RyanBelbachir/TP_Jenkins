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
            echo currentBuild.result
          }
      }
      stage ('Code Analysis') {
          steps {
            withSonarQubeEnv('SonarQube') {
                bat "gradle sonarqube";
            }
            echo currentBuild.result
          }
      }
      stage("Quality gate") {
          steps {
            waitForQualityGate abortPipeline: true;
            echo currentBuild.result
          }
      }
      stage ("Build") {
        steps {
            bat "gradle jar";
            bat "gradle javadoc";
            echo currentBuild.result
        }
      }
      /*stage ("Deploy") {
        steps {
            bat "gradle publish";
        }
      }*/
}

}