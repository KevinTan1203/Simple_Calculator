pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        deleteDir() // Optional: Clear workspace
        git branch: 'main', url: 'https://github.com/KevinTan1203/Simple_Calculator.git'
      } 
    }
    stage('Install Dependencies') {
      steps {
        sh 'python3 -m pip install -r requirements.txt'
      }
    }
    stage('Build') {
      steps {
        echo 'Running calculator script...'
        sh 'python3 calculator.py 1 10 20'
      }
    }
    stage('Test') {
      steps {
        script {
          catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
            echo 'Running unit tests...'
            sh 'python3 -m pytest --maxfail=1 --disable-warnings'
          }
        }
      }
    }
  }
  post {
    success {
      echo 'Build and test stages completed successfully.'
    }
    failure {
      echo 'One or more stages failed. Check the logs for details.'
    }
  }
}
