pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            alaudaStartBuild(buildConfigName: 'hello-world', async: true, branch: 'master', commitID: '4dbbebb1a2e3234d7cfec1e1485d4463fcd3020d')
          }
        }
        stage('') {
          steps {
            sh 'echo XXXXX'
          }
        }
      }
    }
    stage('Parallel Stage') {
      when {
        branch 'master'
      }
      parallel {
        stage('Update Service') {
          steps {
            sh 'Update the service...'
          }
        }
        stage('Deploy an application') {
          steps {
            sh 'Deploy an application...'
          }
        }
      }
    }
    stage('Deploy - Staging') {
      steps {
        echo './deploy staging'
        echo './run-smoke-tests'
        script {
          def browsers = ['chrome', 'firefox', 'opera']
          for (int i = 0; i < browsers.size(); ++i) {
            echo "Testing the ${browsers[i]} browser"
          }
        }
        
      }
    }
    stage('Sanity check') {
      steps {
        input 'Does the staging environment look ok?'
      }
    }
    stage('Deploy - Production') {
      when {
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }
        
      }
      steps {
        echo './deploy production'
      }
    }
  }
  environment {
    DEPLOY_TO = 'production'
  }
  post {
    always {
      echo 'One way or another, I have finished'
      
    }
    
    success {
      echo 'I am successed!'
      
    }
    
    unstable {
      echo 'I am unstable :/'
      
    }
    
    failure {
      echo 'I am failed :('
      
    }
    
    aborted {
      echo 'I am aborted...'
      
    }
    
    changed {
      echo 'Things were different before...'
      
    }
    
  }
  options {
    timeout(time: 1, unit: 'HOURS')
  }
  parameters {
    string(name: 'person', defaultValue: 'sywang', description: 'Who should I say hello to?')
    string(name: 'repoUrl', defaultValue: 'https://github.com/sniperyen/hello-world', description: 'git代码路径')
    string(name: 'repoBranch', defaultValue: 'master', description: 'git分支名称')
    string(name: 'dockerfilePath', defaultValue: 'Dockerfile', description: '相对路径')
  }
}