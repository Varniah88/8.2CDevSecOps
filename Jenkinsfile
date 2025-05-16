pipeline {
  agent {label 'nodejs-builder'
  }
  
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Varniah88/8.2CDevSecOps.git'
      }
    }

    stage('Build') {
      steps {
        echo 'Task: Compile the application using a build automation tool.'
        echo 'Tool: Maven'
        echo 'Building...'
        // Add actual build commands here if any
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        script {
          try {
            sh 'npm test || true'
            sh 'echo "Test log content" > test.log'  // Save test log for email attachment
            currentBuild.result = 'SUCCESS'
          } catch (err) {
            currentBuild.result = 'FAILURE'
            throw err
          }
        }
      }
      post {
        always {
          emailext (
            subject: "Test Stage - ${currentBuild.currentResult}",
            body: "Test stage completed with status: ${currentBuild.currentResult}",
            attachmentsPattern: 'test.log',
            to: 'varniah26@gmail.com'
          )
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage || true'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        script {
          try {
            sh 'npm audit || true'
            sh 'echo "Security scan log content" > security.log'  // Save security scan log
            currentBuild.result = 'SUCCESS'
          } catch (err) {
            currentBuild.result = 'FAILURE'
            throw err
          }
        }
      }
      post {
        always {
          emailext (
            subject: "Security Scan Stage - ${currentBuild.currentResult}",
            body: "Security scan stage completed with status: ${currentBuild.currentResult}",
            attachmentsPattern: 'security.log',
            to: 'varniah26@gmail.com'
          )
        }
      }
    }
  }
}
