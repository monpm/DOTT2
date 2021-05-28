/* since the webhook refuses to work, this line was added to create it from the jenkinsfile*/
properties([pipelineTriggers([githubPush()])])

pipeline {
  agent any
  //{ 
    //docker {                  // we've abandoned this ship, it brought fire and misery (05/28/2021)
      //image 'python:3.7.9'    // this specifies the tests must be run with python 
    //} 
  //}
  stages {
    
    stage('git checkout') {
      steps {
        git changelog: false, poll: false, url: 'https://github.com/monpm/DOTT2.git'
      }
    }
    
    //stage('virEnv') { 
      //steps {
        //sh 'python --version'
      //}
    //}
    
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
    
    //stage('python checkout') {
      //steps {
        //sh 'python --version'
        //sh 'echo $WORKSPACE'
      //}
    //}
    
    //stage('code analysis') {
      //steps {
        //script {
          //def scannerHome = tool 'sonarqubeint';
          //withSonarQubeEnv("sonarqubeint") {
            //sh "${tool("sonarqubeint")}/bin/sonar-scanner \
            //-Dsonar.projectKey=second-test_python-DOTT \
            //-Dsonar.sources=. \
            //-Dsonar.host.url=http://54.82.121.32:9000 \
            //-Dsonar.login=8ae41de7e4681440cac4f4e128b939476dc2d1a0"
          //}
        //}
      //}
    //}
    
    stage('st.code analysis') {
      steps {
        script {
          def scannerHome = tool 'sonarcloudint'; 
          withSonarQubeEnv("sonarcloudint") {
            sh "${tool("sonarcloudint")}/bin/sonar-scanner \
            -Dsonar.organization=monpm \
            -Dsonar.projectKey=monpm_DOTT2 \
            -Dsonar.sources=. \
            -Dsonar.host.url=https://sonarcloud.io \
            -Dsonar.login=2d2de66a8adfd562117ee7a3fd796148d211b8e5"
            //-Dsonar.login=${("sonarcloud-token")}"
          }
        }
      }
    }
    
    stage('testing') {
      steps {
        //dir('/var/lib/jenkins/workspace/DOTT_master/cidr_convert_api/python') //nope nope, jenkins knows trust it
        sh 'echo "Testing"'
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'tests.py'
        }
      }
    }
    
    stage('build py image') {
      steps {
        //dir('/var/lib/jenkins/workspace/DOTT_master/cidr_convert_api/python')
        sh 'docker build --tag python:3.7.9 .'
      }
    }
    
    stage('delete existing container') {                 // for re-building purposes
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'docker stop py'
          sh 'docker rm py'
        }
      }
    }
      
    stage('run py container') {
      steps {
        //dir('/var/lib/jenkins/workspace/DOTT_master/cidr_convert_api/python')
        sh 'docker run --publish 8000:8000 --detach --name rb ruby:1.0'
      }
    }
    
  }
}
