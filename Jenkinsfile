/* since the webhook refuses to work, this line was added*/
//properties([pipelineTriggers([githubPush()])])

pipeline {
  agent any //{ 
    //docker { 
      //image 'python:3.7.9' // this specifies the tests must be run with python
    //} 
  //}
  stages {
    stage('git checkout') {
      steps {
        git changelog: false, poll: false, url: 'https://github.com/monpm/DOTT2.git'
      }
    }
    
    //stage('tryout') {
      //steps {
        //sh 'python --version'
      //}
    //}
    
    //stage('build') {
      //steps {
        //sh 'pip install -r ./requirements.txt'
      //}
    //}
    
    //stage('unit test') {
      //steps {
        //sh 'pytest ./tests.py'
      //}
    //}
    
    stage('code analysis') {
      steps {
        script {
          def scannerHome = tool 'sonarqubeint';
          withSonarQubeEnv("sonarqube-container") {
            sh "${tool("sonarqubeint")}/bin/sonar-scanner \
            -Dsonar.projectKey=second-test_python-DOTT \
            -Dsonar.sources=. \
            -Dsonar.host.url=http://54.82.121.32:9000 \
            -Dsonar.login=8ae41de7e4681440cac4f4e128b939476dc2d1a0
          }
        }
      }
    }
    
  }
}
