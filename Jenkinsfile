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
    stage('Deploy to GitHub Pages') {
        steps {
          withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
            sh """
                npm install -g --silent gh-pages@2.1.1
                git config user.email "kevintanyj@gmail.com"
                git config user.name "KevinTan1203"
                gh-pages --dotfiles --message '[skip ci] Updates' --dist build
            """
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
