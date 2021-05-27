/* since the webhook refuses to work, this line was added*/
properties([pipelineTriggers([githubPush()])])

pipeline {
  agent { 
    docker { 
      image 'python:3.7.9' // this specifies the tests must be run with python
    } 
  }
  stages {
    stage('tryout') {
      steps {
        sh '''
          echo 'maybe si sale(?)'
          '''
      }
    }
    stage('build') {
      steps {
        sh 'pip install -r ./requirements.txt'
      }
    }
    stage('test') {
      steps {
        sh 'pytest ./tests.py'
      }
    }
  }
}
