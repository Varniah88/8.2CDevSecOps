pipeline {
  agent {
    docker {
      image 'node:18'
      args '-u root'
    }
  }

  environment {
    EMAIL_RECIPIENTS = 'varniah26@gmail.com'

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Varniah88/8.2CDevSecOps.git'
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
            sh 'npm test | tee test.log'
          } catch (err) {
            sh 'echo "Test stage failed" >> test.log'
            throw err
          }
        }
      }
      post {
        always {
          archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
          mail to: "${env.EMAIL_RECIPIENTS}",
               subject: "Jenkins: Run Tests - ${currentBuild.currentResult}",
               body: """\
Run Tests stage completed.

Result: ${currentBuild.currentResult}
See attached test log for details.
""",
               attachmentsPattern: 'test.log'
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
            sh 'npm audit | tee audit.log'
          } catch (err) {
            sh 'echo "Security scan failed" >> audit.log'
            throw err
          }
        }
      }
      post {
        always {
          archiveArtifacts artifacts: 'audit.log', allowEmptyArchive: true
          mail to: "${env.EMAIL_RECIPIENTS}",
               subject: "Jenkins: Security Scan - ${currentBuild.currentResult}",
               body: """\
Security Scan stage completed.

Result: ${currentBuild.currentResult}
See attached audit log for details.
""",
               attachmentsPattern: 'audit.log'
        }
      }
    }
  }
}
