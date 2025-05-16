pipeline {
  agent {
    docker {
      image 'node:18'  // or another node version you prefer
      args '-u root'   // run as root (optional, for permissions)
    }
  }
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
stage('SonarCloud Analysis') {
  steps {
    withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
      // If sonar-scanner CLI is not installed on the agent, download and unzip it
      sh '''
      if [ ! -d sonar-scanner ]; then
        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
        unzip sonar-scanner.zip
        mv sonar-scanner-4.8.0.2856-linux sonar-scanner
      fi
      
      ./sonar-scanner/bin/sonar-scanner \
        -Dsonar.login=$SONAR_TOKEN \
        -X
      '''
    }
  }
}

  }
}
