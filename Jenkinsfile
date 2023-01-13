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
      /*stage ("Deploy") {
        steps {
            bat "gradle publish";
        }
      }*/
}

}