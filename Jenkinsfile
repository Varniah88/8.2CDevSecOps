pipeline {
  agent {
    docker {
      image 'node:18'  // or another node version you prefer
      args '-u root'   // run as root (optional, for permissions)
    }
  }
  stages {
        stage('Build') {
            steps {
                echo 'Task: Compile the application using a build automation tool.'
                echo 'Tool: Maven'
            }
        }
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
        sh 'npm test || true'
      }
    }
    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage || true'
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit || true'
      }
    }
        stage('Build') {
            steps {
                echo 'Building...'
                // Your build steps here
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        echo 'Running tests...'
                        // Run your tests and generate logs, e.g., save to test.log
                        sh 'echo "Test log content" > test.log'
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
                        body: """Test stage completed with status: ${currentBuild.currentResult}""",
                        attachmentsPattern: 'test.log',
                        to: 'varniah26@gmail.com'
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    try {
                        echo 'Running security scan...'
                        // Run your security scan and save logs, e.g., security.log
                        sh 'echo "Security scan log content" > security.log'
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
                        body: """Security scan stage completed with status: ${currentBuild.currentResult}""",
                        attachmentsPattern: 'security.log',
                        to: 'varniah26@gmail.com'
                    )
                }
            }
    }
  }
}
