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
      sh '''
        set -x
        echo "Downloading SonarScanner CLI..."
        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
        unzip -q sonar-scanner.zip
        export PATH=$PWD/sonar-scanner-5.0.1.3006-linux/bin:$PATH

        echo "Running sonar-scanner..."
        sonar-scanner -Dsonar.login=$SONAR_TOKEN
      '''
    }
  }
}

  }
}
